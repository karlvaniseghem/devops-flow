<style>
img[alt="DevOps"] {
  max-height: 400px;
}
img[alt="logo"] {
  max-width: 200px;
}
</style>

# Introduction

A samle project to streamline the dev-to-production flow.

![](https://vsrm.dev.azure.com/diekeure-webdev/_apis/public/Release/badge/7c0fb88e-e81b-4a58-beb9-531f522fcd79/10/27)

# Release flow

## Trunk-based branching

> A source-control branching model, where developers collaborate on code in a single branch called ‘trunk’ \*, resist any pressure to create other long-lived development branches by employing documented techniques. They therefore avoid merge hell, do not break the build, and live happily ever after.
>
> \* _master_, in Git nomenclature
>
> -- <cite>[https://trunkbaseddevelopment.com/](https://trunkbaseddevelopment.com/)</cite>

---

Our team uses a trunk-based branching strategy to help developing quickly.

![trunk-based branching](./assets/trunk_based_dev.jpg 'Trunk Based Branching')

Trunk-Based Development is best done with **short-lived feature branches**: one person over a couple of days (max) and **flowing through Pull-Request style code-review & build automation** before merging into the trunk (or master).

### Claims

- You should do Trunk-Based Development instead of GitFlow that feature multiple long-running branches.
- You can either do a Pull-Request workflow as long as those feature branches are short-lived and the product of a single person.

### Releasability of work in progress

Trunk-Based Development will alway be **release ready**.

The rule is to **never break the build** and **always be release ready**.

_<u>Where releases happen</u>_

A key rule is that we make a branch from the trunk specifically for the actual release.

## Development

1. **Branch**  
   The first step when a developer wants to implement a feature is to create a new branch off of the trunk, `master`. We encourage developers to commit early and to avoid long-running feature branches.

2. **Push**  
   When the developer is ready to get their changes integrated, they push their local branch to a branch on the server.  
   Then the developer opens a Pull Request.

3. **Pull Request**  
   When a Pull Request is openend, the proposed changes are build and the repository run a quick test pass.
   We require that other members of the team review the code and approve the changes. Each Pull Request requires 2 approvals before the changes can be merged.  
   If not all tests pass or a reviewer requests changes, the developer has to commit it's changes to the same branch. Again, these changes should be reviewed and approved.

4. **Merge**  
   Once the reviewers approved and all tests passes, then the Pull Request is completed. This means that the topic branch can be merged into the trunk, `master`.  
   After merge, we run additional tests that make more time to run. This gives us a good balance between having fast tests during the Pull Request review yet still having complete test coverage before release.

### Naming Conventions

- **Feature** branches are prefixed with `feature/`.

- **Hotfix** branches are prefixed with `hotfix/`.

- **Release** branches are prefixed with `release/`.

- **Release** branches contains the index of the finished sprint: `release/S29`.

- Spaces in branch names are replaced with dashes (`-`).

## Releases at Sprint Milestones

At the end of the sprint, we create a release branch from the master branch: for example, at the end of sprint S29, we create a new branch `release/S29`. We then put `release/S29` branch into production.

![release branch](./assets/release_branch.jpg 'Release Branch')

Once we've branched to our release branch, the `master` branch remains open for the developers to merge changes. These changes do not get deployed to production.

## Releasing hotfixes

Sometimes we need to bring a bug fix into production to solve critical issues.

When this happens, we start with our normal workflow. We create a branch _from `master`_, get is code reviewed, and complete the pull request to merge it. We always merge it into `master` first. This allows us to create the fix quickly, and validate it locally without having to switch to the release branch locally.

But more importantly, we are guaranteed that our change goes into `master`. This way, we can't accidently forget to keep this hotfix in `master`. Otherwise, we would have a recurrence of the bug during the next deploy.

To bring changes into production, once we have merged the Pull Request into master, we cherry-pick the change into the release branch.

![cherry pick hotfix](./assets/hotfix.jpg 'Hotfix')

## Moving On

After the sprint, we'll finish adding features to sprint `release/S30`. To deploy, we'll create the new release branch, `release/S30` from master, and deploy that.

At this point we have two release branches. If `release/S30` fails in production, we can revert back to `release/S29` in a very ease way.
Once `release/S30` is deployed `release/S29` is completely abandonned. We keep the 5 youngest branches. Older release branches can be deleted safely.

We ensured that any changes that we brought into the sprint `release/S29` as a hotfix were also made in `master`. So those changes will also be in the `release/S30` branch that we create.

# DevOps

> ## The Devops Mindset
>
> Make your software-building process into an automated pipeline and optimize it for speed of delivery.
>
> The software pipeline should ensure security, quality and stability by automating the building and testing of infrastructure and applications and progressively prove their fitness by providing feedback (via tests), and when ready, deploy it to end-users.
>
> Work to continuously improve the flow (speed) and feedback of the software pipeline.  
> -- <cite>[Ash Isaac](https://dev.to/ashokisaac/devops-in-3-sentences-17c4)</cite>

![DevOps](./assets/DevOps.jpg)

## DevOps Fundamentals

DevOps covers a wide range of practices across the application lifecycle. Customers start with one or more of these practices in their journey to DevOps success.

- **Source Code Management** - Teams looking for better ways to manage changes to documents, software, images, large web sites, and other collections of code, configuration, and metadata among disparate teams.
- **Agile Project & Portfolio Management** - Teams looking for a better way of initiating, planning, executing, controlling, and closing the work of a team to achieve specific goals and meet specific success criteria at the specified time.
- **Continuous Integration (CI)** - Teams looking for ways to automate the build and testing processes to consistently integrate code and continuously test to minimise the manual efforts spent in frequent runs of unit and integration tests.
- **Continuous Delivery (CD)**- Teams looking for ways to automate the build, test and packaging, configuration and deployment of applications to a target environment.
- **Shift Left Security** - Teams looking for ways to identify vulnerabilities during development with actionable information to empower dev to remediate vulnerabilities earlier in the life cycleeve specific goals and meet specific success criteria at the specified time.
- **Monitoring and Feedback** - Teams looking for ways to embed monitoring into every deployed version and the impact of application changes to the business value and user experience
- **Rapid Innovation** - Teams looking for ways to provide feedback back into the development, test, packaging & deployment stages to complete the loop to integrate Dev and Ops teams and provide real time feedback from production environments and customers.

## Our DevOps flow
Our implementation of the devops principle is presented in the flowchart below

![Release Pipeline](./assets/pipeline_flow.jpg)

---

![logo](./assets/DK-logo.jpg =75x)
