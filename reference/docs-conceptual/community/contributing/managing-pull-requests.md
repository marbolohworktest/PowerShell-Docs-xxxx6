---
description: This article explains how the PowerShell-Docs team manages pull requests.
ms.date: 07/25/2022
title: How we manage pull requests
---
# Managing pull requests

This article documents how we manage pull requests in the PowerShell-Docs repository. This article
is designed to be a job aid for members of the PowerShell-Docs team. It's published here to provide
process transparency for our public contributors.

## Best practices

- The person submitting the PR shouldn't merge the PR without a peer review.
- Assign the peer reviewer when the PR is submitted. Early assignment allows the reviewer to respond
  sooner with editorial remarks.
- Use comments to describe the nature of the change being submitted. Be sure to @mention the
  reviewer. For example, if the change is minor and you don't need a full technical review, explain
  this in a comment.
- Reviewers should use the comment suggestion feature, when appropriate, to make it easier for the
  author to accept the suggested change. For more information, see
  [Reviewing proposed changes in a pull request][1].

## PR Process steps

1. Writer: Create PR
   - Fill out the [PR template][2]
   - Link any issues resolved by the PR
   - Use GitHub's [autoclose][3] feature to close the issue
   - Work through and check off each item in the checklist
1. Writer: Assign peer reviewer
1. Reviewer: proofreads and comments (as necessary)
1. Writer: Incorporate review feedback
1. Both: Review preview rendering
1. Both: Review validation report - fix warnings and errors
1. Reviewer: Mark review "Approved"
1. Repo Admin: Merge PR (see below for criteria)

## Content Reviewer Checklist

See the [editorial checklist][4] for a more comprehensive list.

- Proofread for grammar, style, concision, technical accuracy
- Ensure examples still apply for the target version
- Check Preview rendering
- Check metadata - ms.date, remove ms.assetid, ensure required fields
- Validate markdown correctness
  - See style guide for content-specific formatting rules
- Reorganize examples as follows:
  - Intro sentence(s)
  - Code and output
  - Detailed explanation of code (as necessary)
- Check hyperlinks for accuracy
  - Replace or remove TechNet/MSDN links
  - Ensure minimum number of redirects to target
  - Ensure HTTPS
  - Correct link type
    - File links for local files
    - URL links for files outside of the docset
  - Remove locales from URLs
  - Simplify URLs pointing to `learn.microsoft.com`
- Verify versioned content is correct across all versions
  - Review the [versioned content change report][5] to see summarized changes

## Branch Merge Process

The `main` branch is the only branch that's merged into `live`. Merges from short-lived (working)
branches should be squashed.

| _Merge from/to_  | _release-branch_ |      _main_      |   _live_    |
| ---------------- | :--------------: | :--------------: | :---------: |
| _working-branch_ | squash and merge | squash and merge | Not allowed |
| _release-branch_ |     &mdash;      |      merge       | Not allowed |
| _main_           |      rebase      |     &mdash;      |    merge    |

### PR Merger checklist

- Content review complete
- Correct target branch for the change
- No merge conflicts
- All validation and build step pass
  - Warnings and suggestions should be fixed (see [Notes][6] for exceptions)
  - No broken links
  - The [Checklist][7] action ran and passed
  - If an [Authorization][8] check was triggered, it passed
- Merge according to table

### Notes

The following warnings can be ignored:

```
Can't find service name for `<version>/<modulepath>/About/About.md`
```

```
Metadata with following name(s) are not allowed to be set in Yaml header, or as file level
metadata in docfx.json, or as global metadata in docfx.json: `locale`. They are generated by
Docs platform, so the values set in these 3 places will be ignored. Please remove them from all
3 places to resolve the warning.
```

When a PR is merged, the HEAD of the target branch is changed. Any open PRs that were based on the
previous HEAD are now outdated. The outdated PR can be merged using Admin rights to override the
merge warnings in GitHub. This is safe to do if the previously merged PRs haven't touched the same
files. Clicking the **Update Branch** button is the safest option. Choose **Update with rebase**
option. For more information see [Updating your pull request branch][9].

## Publishing to Live

Periodically, the changes accumulated in the `main` branch need to be published to the live
website.

- The `main` branch is merged to `live` each weekday at 3pm PST.
- The `main` branch should be merged to `live` after any significant change.
  - Changes to 50 or more files
  - After merging a release branch
  - Changes to repo or docset configurations (docfx.json, OPS configs, build scripts, etc.)
  - Changes to the redirection file
  - Changes to the TOC
  - After merging a "project" branch (content reorg, bulk update, etc.)

<!-- link references -->
[1]: https://docs.github.com/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/reviewing-proposed-changes-in-a-pull-request
[2]: pull-requests.md#use-the-pr-template
[3]: https://help.github.com/en/articles/closing-issues-using-keywords
[4]: editorial-checklist.md
[5]: pull-requests.md#versioned-content-change-reporting
[6]: #notes
[7]: pull-requests.md#checklist-verification
[8]: pull-requests.md#authorization-verification
[9]: https://docs.github.com/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/keeping-your-pull-request-in-sync-with-the-base-branch#updating-your-pull-request-branch