# Contributing to this repository

#### Notes
- ```master``` branch is always the 'current' branch where development for the next minor or major release is taking place
- named branches (e.g. ```liberty-12.1```, or ```mitaka-13.2```) are release branches where releases are tagged
- All branches have gating enabled, and patches should not be merged that do not pass gating
- QE testing is only performed on major (e.g. ```12.0.0```) and minor (e.g. ```12.1.0```) releases, not on patch (e.g. ```12.1.5```) releases, which are assumed to be adequately tested by the commit based, and periodic, jenkins gating.

##### Version definitions

**Major.minor.patch**

* Major
  * openstack release
  * possibly feature?
* minor
  * big change within release
  * security  - upgrade-impacting
  * bug fixes - upgrade-impacting
  * features
  * sha bumps
* patch
  * security  - non-upgrade-impacting
  * bug fixes - non-upgrade-impacting

## Working in the repository

### Filing an issue report

When creating an issue, please ensure you include the following information

* branch (or tag) on which you encountered this issue
* Steps to reproduce
* Any output or logs that you gathered from your environment

### Issue triage
Regular triage meeting are held, their purpose is to provide a time where new issues can be prioritised and the next set of issues to fix can be identified. Each release will target a number of issues, that list will be compiled from the highest priority issues available. Where there are more issues of a particular priority than available spots on the list, the triage meeting provides a place to discuss which issues to select.

Each issue is given a priority and that priority is used to decide the order in which issues are worked. It is important to remember that there is a finite resource available to fix issues. The process of prioritisation is used to focus the available resources on the most important tasks identified. Prioritisation will never be perfect, there will always be competition for resources and there will always be issues addressed slower than an individual would like.

|Priority      |Description                                                                                                                    |Examples                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
|prio-expedited|Any issue that causes a fundamental failure in the product, especially if there is no workaround or it affects all deployments.|Data loss, security bugs, deployment failures of core functionality.|
|prio-1        |Fundamental failures where there is a workaround or only a subset of users are affected.                                       |Incorrect default configurations.                                   |
|prio-2        |Bugs affecting non-standard configurations or that do not directly affect the customer experience or non-core features.        |Individual MaaS plugin failures.                                    |
|prio-3        |Low impact bugs that are cosmetic or cause limited tangible effect.                                                            |Typographical errors.                                               |
|prio-wishlist |Issues that are not bugs but feature request                                                                                   |Additional Logstash filters, MaaS plugin enhancement.               |
|prio-undecided|There is insufficient information on which to determine a priority                                                             |Issues lacking steps to reproduce or logs.                          |


If a 'prio-expedited' issue is identified between triage meetings that will be added to the list at the expense of a lesser priority issue.

'prio-undecided' issues should be reviewed at every triage meeting based on any new information that has been provided.

This description of the triage process is intended to allow flexibility, if you feel an new issue needs expediting or the priority of an issue has changed please feel free to raise it before the next triage meeting.

### Working an issue
#### Selecting an issue to work on
Review the issues with the label 'status-approved'.

Always work on issues with the highest available priority. If there are multiple issues of the same priority, use your judgement when selecting the most appropriate to work on.

Once an issue is selected, assign yourself as the owner and change the status label from 'status-approved' to 'status-doing'.

#### Fixing an issue
##### Gathering information
Issues may or may not contain sufficient information from which to construct a commit. If you require more information update the issue with what you require. In addition, if the issue was created internally, feel free to reach out to the relevant parties directly. GitHub should e-mail the issue creator however do not rely on this method if a lack of information is hindering a resolution.

##### Create pull request
If an issue affects multiple branches always start by creating a pull request for the newest branch, generally this means master. Once that has merged you should be okay to create pull requests for all other affected branches. This is done to prevent the need to review multiple pull requests for the same thing and reduce the chances that the branches will end up with different version of the same commit.

Use the following branch naming scheme when fixing an issue:

issues/issue_id/release_branch

E.g. issues/1234/liberty-12.1

The purpose of having a branch naming convention is to make it easier to correlate branches with their purpose when viewed on the project repo.

When the commit is complete push the branch to rcbops/rpc-openstack

git push upstream issues/issue_id/release_branch

The purpose of pushing the branch directly to rcbops/rpc-openstack is that it allows anyone on the team to update the pull request. If you create a pull request from a branch on your own GitHub repo the only way someone else can modify it is to create a new pull request. Generally this should not be required however there can be times, such as when someone is on leave or an issue needs to be expedited, that it is necessary for someone else to adjust a commit.
Once the branch has been pushed to rpc-openstack, create a pull request. Pull requests are not linked with issues beyond being able to reference one another, this means any required labels providing context to the issue, e.g. priority, need to be added to the pull request separately. Add the issue priority and set the target release label to be the issue target release label for the branch in question, for example if an issue has the labels r12.1.1, and r12.2.0 and the pull request is for the branch liberty-12.1, the label r12.1.1 should be set on the pull request. In addition add the label 'status-needs-review' to the pull request. If the pull request is a work in progress (WIP) make sure to add the label ‘status-dont-merge’.

##### Backports
As the issue owner you are responsible for back porting the commit. As each commit merges, the individual that merges the commit should delete the issue branch using the link on the pull request page and tick the issue checklist for the fixed branch.

##### Closing the issue
An issue should be closed by the individual that merges the commit that fixes that last branch listed as affected by the issue. As the issue owner you are ultimately responsible for it and should validate that all required work is complete if you are not the one to close the issue.

## Commits, pull requests and reviews

### commits

Please avoid the following patterns within individual commits:

* Mixing whitespace changes with functional code changes
* Mixing two unrelated functional changes in the same commit
* Sending large new features in a single giant commit

Expected git commit message structure

* The first line should be limited to 50 characters, should not end with a full stop and the first word should be captialised
* Use the imperative mood in the subject line - e.g. ```Fix a typo in CONTRIBUTING.md```, ```Remove if/else block in myfile.sh```, ```Eat your dinner```
* Insert a single blank line after the first line
* Subsequent lines should be wrapped at 72 characters
* Provide a detailed description of the change in the following lines, using the guidelines in the section below

In your commit message please consider the following points:

* The commit subject line is the most important
* The commit message must contain all the information required to fully understand & review the patch for correctness. Less is not more. More is More
* Do not assume the reviewer understands what the original problem was
* Do not assume the reviewer has access to external web services/site
* Do not assume the code is self-evident/self-documenting
* Describe why a change is being made
* Read the commit message to see if it hints at improved code structure - if so you may be able to split this into two or more commits
* Ensure sufficient information to decide whether to review

### pull requests

* Pull Requests (PRs) should ideally contain a single commit
* The PR title / message should contain information relevant to why the commit(s) are happening. This can be based off the commit message but does not need to be
* The PR description should also include a reference to the original issue
* Where absolutely necessary, related commits can be grouped into a single PR, but this should be the exception not the rule

### reviewing a Pull Request (PR)

When reviewing a PR, please ensure the included commit(s):

* Actually works to fix the issue
* Passes the gate checks that are configured to run
* Contains a single commit, which itself has a good commit message describing the changes involved in the patch (on rare occasion, multiple related commits in the same PR could be considered)
* Does not overreach - each commit should be self contained enough to only address the issue at hand, and no more (see above)

### merging a fix

In order for a fix to be merged, the following criteria should be met:

* All gate tests must have passed
* There must be at least 2 reviewers (members of the rcbops engineering team) who have given it the thumbsup/+1 (using the review guidelines above)
  * If a patch is being backported, the one doing the backport cannot vote on it, but the original author of the patch can
* The second of those +1 reviewers should merge the patch


## release workflow

### major/minor releases
1. work (meaning bugfixes and feature development) is performed in the ```master``` branch in preparation for a major or minor release (e.g. ```12.0.0``` or ```12.2.0```)
2. When all criteria for the targeted release are fulfilled, a release branch is created using the naming convention **series-Major.minor** (e.g. ```liberty-12.0```, or ```mitaka-13.1```), and an rc tag created (e.g. ```r12.0.0rc1```). This tag is then passed to the QE team for initial testing
3. Work continues in ```master``` on features and bugs targeted at the next major or minor release (e.g. ```12.1.0```)
4. As QE (and potentially support and other teams) progress their testing on the release candidate, bugs will be identified in the rc tag that was handed to them. These bugs should be fixed in ```master``` and cherry-picked to the release branch. **No other bug fixes should be cherry-picked into this branch** so that this branch can remain a non-moving target for QE.
5. Once all bugs from the initial release candidate have been cherry-picked into the release branch, a new release candidate should be tagged (e.g. ```r12.0.0rc2```)
6. Steps 4-5 should be repeated until the latest rc passes all QE tests satisfactorily.
7. Once QE are satisfied, a release tag (e.g. ```r12.0.0```) is created in the release branch

### patch releases
1. Work (bugfixes) is performed in the ```master``` branch and cherry-picked into the release branch (e.g. ```liberty-12.1``` or ```mitaka-13.1```). OR work (bugfixes) is performed directly in the release branch if it is release specific and doesn't affect ```master```.
2. Every 2 weeks (approximately) a new release tag (e.g. ```12.1.1```) is made.
3. Immediately after tagging, all external projects included either via submodules, ansible-galaxy or some other mechanism, will have the version/revision/SHA updated to point to the HEAD of that project (in vernacular, we'll do a SHA bump). This allows an immediate set of gate jobs to run on those SHA bumps, as well as the next 2 weeks of development to happen against those new SHA's. This will allow us to stay current and only have to cope with incremental change in external projects. 

[1]
```
sample markdown for checklist

---
- [x] master PR: https://github.com/rcbops/rpc-openstack/pull/{{number}}
- [] kilo PR: https://github.com/rcbops/rpc-openstack/pull/{{number}}
```

