---
layout: default
class: menu-slide
---

<TopStrip :crumbs="['PART 2', 'WHEN CONTEXT GOES WRONG', 'MENU']" />

# PICK A PROBLEM.

<div class="menu-grid">
  <div class="menu-group">
    <Tag text="HOW CONTEXT BREAKS" />
    <ExerciseTile code="E1" name="Forgot" part="P1–2" />
    <ExerciseTile code="E2" name="Buried" part="P2·6" />
    <ExerciseTile code="E3" name="Contradiction" part="P4" />
    <ExerciseTile code="E4" name="Stale" part="P4" />
    <ExerciseTile code="E5" name="Leading" part="P3" />
    <ExerciseTile code="E6" name="Hijacked" part="P2·4" />
    <ExerciseTile code="E7" name="Caved" part="P4" />
  </div>
  <div class="menu-group">
    <Tag text="CONTEXT YOU DIDN'T WRITE" />
    <ExerciseTile code="E8" name="Invention" part="P4" />
    <ExerciseTile code="E9" name="Audited research" part="P4·6" />
    <ExerciseTile code="E10" name="Synthesis" part="P2·6" />
    <ExerciseTile code="E11" name="Hidden context" part="—" />
  </div>
  <div class="menu-group">
    <Tag text="CARRY IT WITH YOU" />
    <ExerciseTile code="E12" name="Handoff" part="P1–6" />
    <ExerciseTile code="E13" name="Inversion" part="P3" />
  </div>
</div>

<style>
.menu-slide h1 { margin-bottom: 40px; }
.menu-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 44px; align-items: start; }
.menu-group { display: flex; flex-direction: column; gap: 12px; }
.menu-group .tag { align-self: flex-start; margin-bottom: 8px; }
</style>

