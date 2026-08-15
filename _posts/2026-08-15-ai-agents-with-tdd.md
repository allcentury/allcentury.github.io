---
layout: post
title: AI Agents, TDD and the path to reliable iterative development
---

For years and years, I have been practicing Test Driven Development (TDD) in various styles and with various languages with different levels of success. In some languages (Ruby I'm looking at you), I don't really understand how people develop without TDD - there are too many gotchas. In other languages like Go or Typescript, I understand why TDD isn't as widely adopted (though I think it's easy to argue that it should be).

The difference in 2026 though is that I've noticed with AI, that it performs much better if we (AI and I - yes that's a we) agree on the tests before it actually implements the code. If AI and I agree on the contract/interface of what we're doing, it rarely gets the implementation wrong and if it does, it catches it before I do.

Let's walk through an example. I asked Claude to implement an `InventoryManager` for a warehouse system in Typescript. First, I didn't specify TDD - I just asked for the code.

## Without TDD

The prompt:

> Implement an InventoryManager class in Typescript. It should support adding stock for a SKU, removing stock for a SKU, and reading the current stock level for a SKU.

Here's what came back:

```typescript
class InventoryManager {
  private stock = new Map<string, number>();

  addStock(sku: string, qty: number): void {
    this.stock.set(sku, (this.stock.get(sku) ?? 0) + qty);
  }

  removeStock(sku: string, qty: number): void {
    const current = this.stock.get(sku) ?? 0;
    this.stock.set(sku, current - qty);
  }

  getStock(sku: string): number {
    return this.stock.get(sku) ?? 0;
  }
}
```

Looks reasonable. It reads cleanly, the naming is good, and if you poke at it in a REPL for a minute it behaves exactly like you'd expect. This is also exactly the kind of code that gets rubber stamped in a PR review, it's short, it's obviously "correct" at a glance.

Except it isn't. Nothing stops `removeStock` from taking a SKU negative, and nothing stops you from removing stock for a SKU that was never added in the first place:

```typescript
const inv = new InventoryManager();
inv.addStock('sku-1', 2);
inv.removeStock('sku-1', 5);
inv.getStock('sku-1'); // -3, we just sold stock we don't have

inv.removeStock('sku-unknown', 1);
inv.getStock('sku-unknown'); // -1, a SKU that never existed now has negative stock
```

That's an oversell bug, and it happens silently, no error, no exception, nothing in the logs. I didn't catch this by reading the code, I caught it because I went looking for it, and I only went looking because I've been burned by this exact class of bug before. That's the problem with reviewing AI generated code by reading it: you're relying on your own experience to know what questions to ask, and the code always *looks* finished.

## With TDD

Same ask, but this time I told Claude to agree on the tests with me before writing any implementation.

The prompt:

> Let's implement an InventoryManager class in Typescript, but write the tests first. It should support adding stock for a SKU, removing stock for a SKU, and reading the current stock level. We should agree on happy path cases and unhappy path and there should be tests for both. Lets align on the tests one by one before writing the implementation.

The tests Claude proposed:

```typescript
describe('InventoryManager', () => {
  it('adds stock for a new sku', () => {
    const inv = new InventoryManager();
    inv.addStock('sku-1', 10);
    expect(inv.getStock('sku-1')).toBe(10);
  });

  it('removes stock down to zero', () => {
    const inv = new InventoryManager();
    inv.addStock('sku-1', 10);
    inv.removeStock('sku-1', 4);
    expect(inv.getStock('sku-1')).toBe(6);
  });

  it('throws when removing more than is in stock', () => {
    const inv = new InventoryManager();
    inv.addStock('sku-1', 2);
    expect(() => inv.removeStock('sku-1', 5)).toThrow('Insufficient stock');
  });

  it('throws when removing stock for an unknown sku', () => {
    const inv = new InventoryManager();
    expect(() => inv.removeStock('sku-unknown', 1)).toThrow('Unknown sku');
  });
});
```

That's where the actual value is for me, reading through four short test cases before any implementation exists. The oversell case and the unknown SKU case are right there in the contract, in plain english, before a single line of implementation is written. I didn't have to think of them from scratch, I just had to read four short tests and ask "does this cover it?" That's a much easier review than reading an implementation and trying to imagine everything it *doesn't* handle.

Once I confirmed the tests, the implementation:

```typescript
class InventoryManager {
  private stock = new Map<string, number>();

  addStock(sku: string, qty: number): void {
    this.stock.set(sku, (this.stock.get(sku) ?? 0) + qty);
  }

  removeStock(sku: string, qty: number): void {
    if (!this.stock.has(sku)) {
      throw new Error('Unknown sku');
    }

    const current = this.stock.get(sku)!;
    if (qty > current) {
      throw new Error('Insufficient stock');
    }

    this.stock.set(sku, current - qty);
  }

  getStock(sku: string): number {
    return this.stock.get(sku) ?? 0;
  }
}
```

Same class, same three methods, but this version can't oversell and can't silently invent negative stock for a SKU that doesn't exist. The interesting part isn't that the second version is better code, it's *why* it's better. Nothing about the second prompt was smarter than the first one, I didn't describe the bug or tell it to be careful about edge cases. I just made it commit to a contract before it committed to an implementation, and that contract is what forced the oversell case into the open.

## Why this works

When you just ask for the implementation, AI's only goal is code that looks like it does the thing. When you ask for tests first, you're forcing it to answer a different question before it's allowed to write the class: what does "correct" actually mean here? The oversell case and the unknown SKU case showed up in the tests not because I told it to think about edge cases, they showed up because it had to spell out what happens when `qty` is too high before it could touch the implementation. Once I'd agreed to those four tests, writing the implementation was basically mechanical, satisfy the tests, and there wasn't much room left for either of us to hand-wave past a case neither of us had thought about yet.

This isn't a new idea, it's just TDD. What's changed for me in 2026 is who I'm doing it with. I've always trusted TDD to catch bugs I'd otherwise only find with judgement and experience, turns out that same discipline is just as valuable, maybe more, when the one writing the implementation is an agent instead of me. I can't exactly lean on "I remember writing this part carefully" anymore.
