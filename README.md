# ciclops

ciclops (CI-clops) is a GitHub Action that summarizes the result of a Test Suite
executed in a series of "strategy matrix" branches, and helps direct
investigations when tests fail.

The legends tell of a mythical one-eyed creature that was cursed by the gods to
watched over Continuous Integration pipelines for all eternity.

## Origin

At EDB, working on a series of Kubernetes operators for PostgreSQL, we have an
extensive test suite that is executed for a variety of combinations of
PostgreSQL and Kubernetes versions, using GitHub's "strategy matrix" step.

When there are any failures in a given run of our CI/CD pipeline, it becomes
difficult to make sense of things. With so many tests executed in so many matrix
branches, clicking into each matrix branch an]d drilling in can become painful,
and systemic issues may escape scrutiny, buried in data.

ciclops adds a Markdown summary to the GitHub Actions output of our CI/CD step,
and buckets tests according to several different criteria, doing the grunt work
of figuring out if there was a pattern to failures.
