---
layout: default
class: menu-slide
---

<TopStrip :crumbs="['PART 2', 'WHEN CONTEXT GOES WRONG', 'MENU']" :links="[{ label: 'BRIDGE', to: 'bridge' }, { label: 'ENDING', to: 'ending' }]" />

<!-- Highlighted tiles: on the first menu they appear on the first click; on later menus they show straight away. -->

# PICK A PROBLEM.

<div class="menu-grid">
  <div class="menu-group">
    <Tag text="HOW CONTEXT BREAKS" />
    <ExerciseTile code="E1" name="Forgot" :featured="$frontmatter.routeAlias !== 'menu' || $clicks >= 1" />
    <ExerciseTile code="E2" name="Buried" />
    <ExerciseTile code="E3" name="Contradiction" />
    <ExerciseTile code="E4" name="Stale" />
    <ExerciseTile code="E5" name="Leading" />
    <ExerciseTile code="E6" name="Hijacked" :featured="$frontmatter.routeAlias !== 'menu' || $clicks >= 1" />
    <ExerciseTile code="E7" name="Caved" />
  </div>
  <div class="menu-group">
    <Tag text="CONTEXT YOU DIDN'T WRITE" />
    <ExerciseTile code="E8" name="Filled in" />
    <ExerciseTile code="E9" name="Audited research" :featured="$frontmatter.routeAlias !== 'menu' || $clicks >= 1" />
    <ExerciseTile code="E10" name="Synthesis" />
    <ExerciseTile code="E11" name="Hidden context" :featured="$frontmatter.routeAlias !== 'menu' || $clicks >= 1" />
  </div>
  <div class="menu-group">
    <Tag text="CARRY IT WITH YOU" />
    <ExerciseTile code="E12" name="Handoff" :featured="$frontmatter.routeAlias !== 'menu' || $clicks >= 1" />
    <ExerciseTile code="E13" name="Inversion" />
  </div>
</div>

<style>
.menu-slide h1 { margin-bottom: 40px; }
.menu-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 44px; align-items: start; }
.menu-group { display: flex; flex-direction: column; gap: 12px; }
.menu-group .tag { align-self: flex-start; margin-bottom: 8px; }
</style>

