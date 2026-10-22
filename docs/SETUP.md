# Publish your GitHub profile

## 1. Upload the profile

1. Sign in as **lamidu-rathnayake** and create a **public** repository named exactly **lamidu-rathnayake**. Use `main` as the default branch. If this repository already exists, update it instead of creating another one.
2. Extract the supplied ZIP. Copy the **contents** of its `lamidu-rathnayake` folder into the repository root, including the hidden `.github` folder. The root must contain `README.md`, not another enclosing profile folder.
3. Commit and push the files to `main`. The README appears on [your GitHub profile](https://github.com/lamidu-rathnayake).

GitHub requires a public repository with the same name as the account and a nonempty root README. [Profile README requirements](https://docs.github.com/en/account-and-profile/how-tos/setting-up-and-managing-your-github-profile/customizing-your-profile/managing-your-profile-readme)

## 2. Turn on the activity cards

1. Open the repository's **Settings → Actions → General**. Ensure Actions are enabled and the actions used by the workflow are permitted. On an unrestricted personal repository, the normal Actions setting is sufficient.
2. Open **Actions → Update profile cards → Run workflow**. Select the default branch (`main`) and run it.
3. Wait for a successful run, then refresh your profile. GitHub image caching can delay the visible update.

**No personal access token or custom secret is needed.** GitHub supplies `GITHUB_TOKEN` automatically. The workflow requests only `contents: write` for its update job so it can commit the generated SVGs. You do not need to enable “Allow GitHub Actions to create and approve pull requests.” [GitHub token documentation](https://docs.github.com/en/actions/concepts/security/github_token)

The delivered cards say **pending first update** until the first successful run. The workflow has been prepared for your repository; it has not been executed on your account.

## How updates work

| Item | Behavior |
| --- | --- |
| Schedule | Daily at **01:23 UTC / 06:53 Sri Lanka time**; GitHub may delay scheduled runs. |
| Other triggers | Manual run; changes to the workflow on `main`. |
| Generated files | `assets/generated/stats-dark.svg`, `stats-light.svg`, `languages-dark.svg`, `languages-light.svg`. |
| Branch | Reads and writes the repository's default branch; manual runs selected on other branches are skipped. |
| Failed generation | No update is committed; the last successful cards remain visible. |
| Unchanged data | No extra commit. |
| Credentials | Built-in short-lived token; public statistics only. |

The generator is [GitHub Readme Stats Action](https://github.com/stats-organization/github-readme-stats-action), using the maintained [GitHub Stats Extended](https://github.com/stats-organization/github-stats-extended) renderer. The legacy `anuraghazra/github-readme-stats` project now directs users to these successors. [Migration notice](https://github.com/anuraghazra/github-readme-stats)

Activity statistics describe public GitHub activity. The language card shows the distribution of code bytes in owned, non-forked public repositories; it is **not a proficiency score**, and does not represent private banking work or all contributions to other people's repositories. Rank is hidden. [Language calculation](https://github-stats-extended.vercel.app/frontend/docs/cards/top-languages/)

## Edit and maintain

| Change | File |
| --- | --- |
| Bio, links, stack, project and roadmap | `README.md` |
| Profile artwork | SVG files in `assets/` |
| Schedule, card colors or statistics | `.github/workflows/profile-cards.yml` |
| Automated action update proposals | `.github/dependabot.yml` |

Action dependencies are pinned to verified full commit SHAs. Dependabot checks those references monthly and opens update pull requests for you to review. The renderer is separately fixed to `core_version: '2.1.3'`; review upstream releases before updating all four occurrences. Dependabot's GitHub Actions updater does not update that input. [Keeping actions updated](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/keeping-your-actions-up-to-date-with-dependabot)

If your existing default branch is not `main`, change the `push.branches` entry to its name. Scheduled and manual runs already use the default branch automatically. Keep the workflow file on that default branch.

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| README is absent from the profile | Check the exact repository name, public visibility and root `README.md` location. |
| Images are missing | Upload the complete `assets` directory and preserve paths and capitalization. |
| No “Run workflow” button | Commit `.github/workflows/profile-cards.yml` to the default branch and enable Actions. |
| Push fails with 403 | Read the run log; confirm `contents: write` remains in the job and that repository or organization policy permits it. |
| Push is rejected by branch protection | This workflow commits directly. Use a repository policy that permits this automation, or adapt publishing to your required pull-request process. Do not force-push. |
| Cards still show “pending” | Open the failed run's generation log, resolve the reported API or dependency issue, then rerun the workflow. |
| Update fails during concurrent edits | Rerun after your edits are pushed; the workflow never force-pushes over them. |
| Cards stop refreshing after inactivity | Public repository schedules can be disabled after 60 days without repository activity. Re-enable the workflow in Actions and run it manually. |

Schedules are best effort and run from the default branch. [GitHub scheduling behavior](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule)
