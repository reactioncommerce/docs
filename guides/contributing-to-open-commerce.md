## At A Glance

At Mailchimp Open Commerce, we're dedicated to the open source community. In fact, we've designed our entire platform to grow from the passion and creativity that an open source community ignites. We've already attracted a small, dedicated team of open source contributors, and there's always room for more. If you'd like to join us, here's how to get started.

## What you need

Anyone can contribute, you just need a GitHub account, and the willingness to make the world a better place through open source.

## Find or Open an Issue

If we are talking about bugs, there are two places to start. Either find an existing open issue, or open a new issue.
You can search issues by going to "Issues" on the appropriate repo and using the search box. Doing this is really 
critical as opening multiple issues on the same doesn't allow us to see that this issue is impacting multiple users 
which may change its priority.

If you find that no one has opened an issue then you can create a new one. Be sure to fill out the entire issue 
template or your issue may be closed due to lack of information. Filling only the subject and none of the template 
almost guarantees that your issue will be closed immediately.

If you want to add a new feature we also ask that you open an issue so that we have a clear understanding of what 
problem you are wanting to solve. Issues are great places to have discussions about implementation and approach 
whereas PRs are not.


## Prepare a Pull Request

**When at all possible avoid breaking changes without prior discussion or your PR will be kept on ice until a new 
major release is approved.**

Make sure you branch off `trunk` to create your PR.

**Use Git commit message conventions**

You will need to follow the [Git Style guide](../docs/git-style-guide/) for both your branch name and your commits. 
We use the [semantic release](https://github. com/semantic-release/semantic-release) to manage versioning so these commit messages are critical and not using the 
correct format will delay getting your PR merged.

**Give your PR a good title**

Make sure the PR title completely describes the new feature or bug fix.

**Fill out the pull request template**
Each of the items there is critical for us for to evaluate the code in your PR. If a section is completely 
irrelevant (e.g. Testing instructions for fixing a typo in the docs) just remove it.

## Pass all tests

We will never merge a PR that doesn't pass build checks and tests. You should be able to run most tests locally by 
just running `npm run test`. Typically we won't even begin the review process 

## Review Process Begins

The team triages all new pull requests as soon as the PR is complete.

**PR gets reviewed**

The team reviews code quality rules including:

  * **PR template**: If the PR doesn't follow our template, reject and point the author of the PR to this doc.
  * **Issue description**: Use this information as the starting point for your review. If something is not clear, reject the PR and ask for clarity by requesting changes. While the original issue may have useful information, the PR should contain the most up to date representation of the issue.
  * **Solution**: Use this information to help determine a path to test this PR. Research any included packages or 
     techniques that may have been used that you're not familiar with. Ask questions if you're confused.
  * **Breaking changes**: Test by applying this patch to an existing install of Reaction with existing users, orders, 
    carts, etc. Specifically, test any parts of the app where the breaking change is involved and any data set that is involved in a migration.
  * **Testing**: Run through the author's steps to verify that it works as they've tested it. Then run through the app 
    on your own as you would test it. Run through the app as many times as you feel comfortable before approving or requesting changes.
  * **Readability**: The linter will help with this, but call out anything that is difficult to understand or that you 
    feel needs comments 
  * **Documentation**: all code added or touched should have proper JSDoc, any new functionality should be documented, as outlined in JSDoc Style Guide. 
  * **Security**: Code should only be usable by users with the correct permissions. Any data published should be 
    filtered to ensure that only users with the correct permissions for the correct shops have access to it. Synk should not fail. Any failing automated tests should not be approved.
  * **Performance**: Code should be written with performance in mind. Data publications should only publish data 
    necessary to accomplish the specific goal at hand. 
  * **Tests**: Any new functionality should include tests
  * **Dependencies**: Any newly introduced dependencies should be updated to the latest version. 
  * **Linting**: Pass all linting tests. If there are minor linting errors, the reviewer may fix them for speed.

Reviewers will note any changes that they will want to QA in the app, even if they aren't listed in the testing steps (e.g if the code changes a cart button, ensure that the button still works).


## Congrats! It's approved. What happens next?
The Open Commerce team reviewer is responsible for merging the PRs they approved, unless the PR submitter has requested 
otherwise.

Now that your PR is merged and it's not a breaking change, the feature will be released in the next release. 
