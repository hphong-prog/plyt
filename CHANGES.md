# Plyt Budget member alignment

- Budget participant selectors and balance rows now use the Plan roster (plan owner + travel-mates).
- Existing legacy expense/payment participants are promoted once into the Plan travel-mate list so previous financial calculations are preserved rather than silently dropping old people/amounts. Their labels are normalized to the canonical plan-member name.
- A member cannot be removed from the plan while existing expenses, custom splits, or settlements still refer to them. Reassign/edit/delete those Budget records first.
- Applied to both `index.html` and `m.html`; Firebase schema, Personal Space and Join Plan flow are unchanged.
