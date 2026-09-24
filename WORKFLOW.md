![alt text](image.png)
![alt text](image-1.png)
![alt text](image-2.png)
![alt text](image-3.png)
![alt text](image-4.png)
![alt text](image-5.png)
![alt text](image-6.png) ![alt text](image-7.png) ![alt text](image-8.png)
![alt text](image-9.png)


**1. Walk through the final `calculateLateFee` function and name which contributor's change is responsible for each part.**

The final function has four pieces, each from a different task. First, `if (daysLate <= 1) { return 0; }` is the grace period, added in Task 1 (Clone A) — no fee for the first day. Next, `Math.round(daysLate * ratePerDay)` is the rounding change from Task 2/3 (Clone B), replacing the original `Math.floor`. Then `Math.min(fee, 20)` is the $20 maximum cap from Task 4/5 (Clone C). Finally, `Math.max(fee, 1)` is the $1 minimum fee from Task 6 (Clone A, added via rebase).

**2. Compare Task 3's two-way conflict to Task 5's three-way conflict — what got harder with a third line of work?**

In Task 3, there were only two versions of the function to reconcile — one change (grace period) and one change (rounding) — so combining them into a single `if` plus one calculation was straightforward. In Task 5, the incoming change already had two behaviors merged together (grace period + rounding), and I had to fold in a third, independent change (the fee cap) without losing either of the first two. There was more logic to track at once, and more risk of silently dropping a piece of someone's work while reading through both sides of the conflict.

**3. What's the actual difference between how you resolved Task 5 (merge) and Task 6 (rebase)?**

`git merge` (Task 5) created a new merge commit with two parents, preserving both branches' histories side by side and joining them together. `git rebase` (Task 6) didn't create a merge commit at all — it replayed my commit as if I'd made it after everyone else's work, rewriting the commit's base entirely. The end file content ended up similar either way, but the commit *history* looks different: merge keeps a branching history with a visible merge commit, while rebase keeps one straight, linear line of commits.

**4. If this were a real team of three, what one process change would have prevented all three rejected pushes?**

Requiring everyone to `git fetch` (and pull in the latest changes) before starting new work — and again right before pushing — would have prevented all three rejections. Each person would have seen the others' changes early and resolved small conflicts as they went, instead of everyone working blind off stale code and colliding at push time.
