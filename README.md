# su26-ai301-contribution

# Contribution [1]: [schema_validation.sh should automatically revert the patch it applies]

**Contribution Number:** [1]  
**Student:** [Praise Olatide]  
**Issue:** [https://github.com/wesnoth/wesnoth/issues/9967#issuecomment-4650546668]  
**Status:** [Phase I [In Progress]

---

## Why I Chose This Issue

Issue seems fairly straight forward with necessary context. Had alot of the green flags like few comments, was an open issue that was still relevant, and fit with the scope of what I already knew. 
---

## Understanding the Issue

### Problem Description
The current validation script (schema_validation.sh) applies a temporary patch but does not revert it. This leaves edited schema files in the working tree aand can be annoying if you commit the edited schema files. 

### Expected Behavior

Running the script should leave the repo in the same state it started in. 

### Current Behavior

The script calls schema_validation.paatch and then runs validation. it never undoes the patch. 

### Affected Components

schema_validation.sh, schema_validation.patch ,  schema files under dataa/schema/... 
---

## Reproduction Process

### Environment Setup

 install dependencies with homebrew boost, sdl3, sdl3_image, sdl3_mixer, fontconfig, cairo, pango, libvorbis, openssl@3, etc.). See SConstruct and INSTALL.md for details.
 
### Steps to Reproduce
starting from a clean checkout with no staged changes. Build the project after installing the necessary dependencies to ensure the ./wesnoth exists. after run the schema_validation.sh file. after it finishes run git status. observe the changed files.

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]
<img width="1178" height="510" alt="image" src="https://github.com/user-attachments/assets/819512de-e53c-483f-a71c-8db30f9c827b" />

---

## Solution Approach

### Analysis

The script runs many validation commands that can fail, but there is no guaranteed cleanup on exit/failure
### Proposed Solution

Run the validation in a temporary work tree so the maain worktree is never modified. 
### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** script applies a patch but does not revert it

**Match:** 

**Plan:** [Step-by-step implementation plan]
1. modiy schema_validation.sh to check for a clean working tree. 
2.implement the worktree flow: create a temp dir, run validations
3. add a fallback trap to revert in-place if worktree is unavailable
4. add logging and exit codes so CI still fails when validation fails
5. add tests 

**Implement:** https://github.com/Praise-creator/wesnoth-ai301/tree/fix-issue-schema_validation-auto-revert 

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** run git status before and after running scropt, must be identical. 

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Run `utils/CI/schema_validation_regression.sh` with a dummy `wesnoth` exe and verify it exits 
- [ ] Test case 2: confirm `git status --porcelain` in the main repository is unchanged before and after the regression script
- [ ] Test case 3: confirm the temp worktree created by the regression test is cleaned up on exit even if validation fails


### Integration Tests

- [ ] Integration scenario 1:run `utils/CI/schema_validation.sh` from inside a temporary  `git worktree` and verify the temporary schema patch stays in thaat worktree


## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [`utils/CI/schema_validation.sh`](utils/CI/schema_validation.sh), [`utils/CI/schema_validation_regression.sh`](utils/CI/schema_validation_regression.sh), [`.github/workflows/ci-main.yml`](.github/workflows/ci-main.yml)
- **Key commits:** 
- **Approach decisions:** used a temporary detached `git worktree` instead of in-place patch so the validation patch cannot leak into the main checkout

---

## Pull Request

[**PR Link:** [GitHub PR URL when submitted]
](https://github.com/wesnoth/wesnoth/pull/11337)
pr for issue #9967
Changes made:
This changes schema_validation.sh so schema validation runs inside a temporary detached git worktree instead of the developer’s main working tree. Regression test was also added in schema_validation_regression.sh


**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged] awaiting review 

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
