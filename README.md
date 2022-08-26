# CIclops

CIclops is a GitHub action that summarizes the result of a Test Suite executed
in a series of "strategy matrix" branches, and helps direct investigations when
tests fail.

## Origin

At EDB, working on a series of Kubernetes operators for PostgreSQL, we have an
extensive test suite that is executed for a variety of combinations of
PostgreSQL and Kubernetes versions, using GitHub's "strategy matrix" step.

When there are any failures in a given run of our CI/CD pipeline, it becomes
difficult to make sense of things. With so many tests executed in so many matrix
branches, systemic issues can become hidden.

CIclops adds a Markdown summary to the GitHub Actions output of our CI/CD step,
and buckets tests according to several different criteria.
