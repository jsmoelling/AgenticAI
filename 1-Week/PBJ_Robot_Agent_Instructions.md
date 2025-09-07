# PB&J Robot Program — Agent Instructions (Classroom Version)

**Purpose:** A literal, robot-ready plan for making one peanut‑butter‑and‑jelly sandwich. Designed for students to see how precise an agent’s instructions must be.

---

## Goal
Make **one** peanut-butter-and-jelly sandwich on a plate.

## Assumptions (Robot Abilities & Tools)
- Vision to identify: **plate, bread bag, bread slice, peanut butter jar, jelly jar, knife, napkin**  
- Two-finger gripper (or suction) and a wrist that can rotate  
- Can apply light force (spread) and measure contact  
- Workspace: a counter with all items visible

## Preconditions & Safety
1. If *peanut allergy* signal == **true** → **ABORT** and announce: *“Allergy detected.”*
2. Confirm all required items are present on the counter. If any missing → **announce which item** and stop.
3. Sanitize tool: pick up napkin, wipe knife, place napkin aside.

---

## High-Level Plan (Explicit, Robot-Sized Steps)

### A) Place Plate & Get Bread Ready
1. Locate **plate** → grasp → place at workspace center.
2. Locate **bread bag** front opening area.
3. If bag has **twist tie/clip**:  
   a. Grasp tie/clip; if tie: rotate counter‑clockwise until loose; if clip: squeeze and pull off.  
   b. Place tie/clip to top‑left of plate.
4. With two fingertips, pinch bag opening; open by pulling sides apart.
5. Insert gripper, grasp **one bread slice** near its top edge; lift vertically; place **slice A** on left side of plate, face up.
6. Grasp **second bread slice** the same way; place **slice B** on right side of plate, face up.
7. Squeeze bag to remove air; close bag; re‑attach tie/clip; place bag aside.

### B) Open Peanut Butter & Jelly
8. Locate **peanut butter jar**; stabilize jar with one gripper on body.
9. With other gripper on lid, rotate lid counter‑clockwise until free; place lid, *inside‑up*, at top‑right of plate.
10. Repeat for **jelly jar**; place that lid next to peanut butter lid.

### C) Peanut Butter Application
11. Locate **knife**; grasp handle with blade pointing away from the robot.
12. Move knife over peanut butter jar opening; lower tip into jar until contact detected.
13. Scoop by dragging knife along surface while rotating wrist; target ~**1 tbsp** (≈15 mL). If insufficient mass (vision check) → scoop again.
14. Move to **slice A** (left). Lower knife until light contact with bread; angle blade ~**15°**.
15. Spread from center to top edge in straight line; lift; return to center.
16. Spread from center to bottom edge; lift.
17. Spread from center to left edge; then center to right edge.
18. Inspect coverage: if **≥90%** of upper surface is covered to within ~**5 mm** of crust → continue; else repeat small fills on uncovered regions.
19. Scrape any excess from knife back into jar rim area to reduce mess.

### D) Jelly Application
20. Move knife tip into **jelly jar**; scoop ~**1 tbsp** (semi‑transparent red/purple mass).
21. Move to **slice B** (right). Repeat spread pattern (steps 15–18) for jelly until coverage **≥90%**.
22. If jelly thickness visibly > peanut butter thickness by **>2×** → scrape small amount back to jar to avoid overflow.

### E) Combine Slices
23. Rotate wrist so knife blade is parallel to plate; lift knife; place it on **napkin** (blade away from edge).
24. Hover over **slice B**; verify jelly on top (shiny).
25. Grasp **slice B** at its left and right edges using gentle pressure.
26. Lift **slice B** vertically 5–8 cm; translate over **slice A**.
27. Lower **slice B** until contact; align edges so crusts match within ~**5 mm**.
28. Release. Apply gentle palm pressure on top (**≤3 N**) to adhere layers.

### F) Cut (Optional)
29. Re‑grasp **knife**. Align blade through sandwich center along shortest axis.
30. Press down while sawing with small forward‑back motion until blade passes through; withdraw blade.
31. Rotate sandwich halves **90°** if triangles desired; repeat cut.

### G) Close & Cleanup
32. Re‑seal **peanut butter jar**: place lid; rotate clockwise until snug.
33. Re‑seal **jelly jar** similarly.
34. Wipe knife with napkin; place knife either in sink zone or on napkin, blade facing inward.
35. Move any crumbs into a small pile with napkin; dispose in trash area.
36. Present sandwich centered on plate; rotate plate so cut faces toward the user.
37. Announce: **“Peanut butter and jelly sandwich complete.”**

---

## Example Tiny Subroutine (Teach Literal Thinking)

**Open a threaded lid (counter‑clockwise):**
1. Place gripper **A** on jar body; apply downward force **5 N** (stabilize).
2. Place gripper **B** on lid edges; close gripper until secure (no slip).
3. Rotate wrist **B** by **−30°**; if slip detected → increase grip by **5%**.
4. Repeat rotation in **−30°** increments until **lid height** increases by **≥2 mm** (thread disengaged).
5. Lift lid vertically; place in designated “lid zone,” *inside facing up*.

---

## Failure Handling (Kid‑Friendly Checks)
- **No bread detected** → say *“Missing bread,”* point to expected location, stop.
- **Peanut butter/jelly too stiff** → increase scoop force a small step; if still stiff, prompt: *“Please stir contents.”*
- **Bread tearing while spreading** → reduce downward force by **30%**, increase number of passes.
- **Sticky knife mess** → use rim‑scrape routine before next spread.

---

### Classroom Tips
- Print this as a **checklist** and have students act as the “robot,” following only what’s written.
- Ask students to add missing edge cases (e.g., *bread upside down*, *jar foil seal*, *crust removal*).
- Compare human‑style vs. robot‑style instructions to illustrate **agent precision**.
