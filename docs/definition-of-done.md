# Definition of Done

A story is **Done** only when all of these are true:

**Built**
- [ ] All acceptance criteria are met
- [ ] Code is merged to `main` through a reviewed PR (self-review with the checklist counts for a solo project)
- [ ] No new lint or TypeScript errors

**Tested**
- [ ] Pure logic in `lib/` has unit tests, including edge cases
- [ ] CI is green
- [ ] Tested on a real Android phone
- [ ] Maestro flow added or updated if a user flow changed
- [ ] Queries ignore soft-deleted rows; undo leaves counts, streak and XP unchanged

**Quality**
- [ ] Accessible: screen-reader labels, contrast ≥ 4.5:1, touch targets ≥ 44px, Reduce Motion respected
- [ ] Safety rules from `CLAUDE.md` followed: no diagnosis, no dose advice, red-flag and chest-pain messages intact
- [ ] No secrets, keys or real health data committed

**Recorded**
- [ ] RTM updated: requirement → test case → result
- [ ] CHANGELOG updated if users will notice the change
- [ ] Issue closed and moved to **Done** on the board
