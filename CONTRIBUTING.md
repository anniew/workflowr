# Contributing

Thanks for your interest in contributing to workflowr.
Here are some guidelines to help make it easier to merge your Pull Request:

* For potentially large changes, please open an Issue first to discuss
* Please submit Pull Requests to the "dev" branch
* Please follow the [Hadley style guide][style]
* Run `devtools::test()` to run the tests
* (Optional) Execute the file [document.R](document.R) to update the
documentation

If you're new to submitting Pull Requests, please read the section [Contribute
to other projects][contribute] in the tutorial [A quick introduction to version
control with Git and GitHub][git-tutorial].

## Explanation of branches

branch name   | purpose
------------- | -------------
master        | stable branch for end users
dev           | development branch - submit Pull Requests here

## More about this repository

For the most part, I try to follow the guidelines from [R packages][r-pkg] by
[Hadley Wickham][hadley]. The unit tests are performed with [testthat][], the
documentation is built with [roxygen2][], the online package documentation is
created with [pkgdown][], continuous integration testing is performed for Linux
and macOS by [Travis CI][travis] and for Windows by [AppVeyor][appveyor], and
code coverage is calculated with [covr][] and [Codecov][].

The template files used by `wflow_start()` to populate a new project are defined
in the list `templates` in the file `R/infrastructure.R`. The [RStudio project
template][pt] is configured by `inst/rstudio/templates/project/wflow_start.dcf`.
The repository contains the files `LICENSE` and `LICENSE.md` to both adhere to
[R package conventions for defining the license][r-exts-licensing] and also to
make the license clear in a more conventional manner (suggestions for
improvement welcome). `document.R` is a convenience script for regenerating the
documentation. `build.sh` is a convenience script for running `R CMD check`. The
remaining directories are standard for R packages as described in the manual
[Writing R Extensions][r-exts].

## Release checklist (for maintainers)

* Bump version in [DESCRIPTION](DESCRIPTION), [NEWS.md](NEWS.md), and
[the test _workflowr.yml file](tests/testthat/files/test-wflow_update/post/_workflowr.yml)
* Bump date in [DESCRIPTION](DESCRIPTION)
* Update [NEWS.md](NEWS.md): Check `git log` and make sure to reference GitHub
Issues/PRs
* Run [document.R](document.R) to update Rd files, install the package locally,
and build the online documentation with [pkgdown][]
* Run [build.sh](build.sh) to confirm tests pass locally
* Test on [rhub][]:
    * Have to validate email first with `rhub::validate_email()`. Copy-paste
    token from email into R console.
    * Check on Ubuntu with `rhub::check_on_ubuntu()`
    * Check on macOS with `rhub::check_on_macos()`
    * Check on Windows with `rhub::check_on_windows()`
* Test on [winbuilder][]:
    * Check with R release with `devtools::check_win_release()`
    * Check with R devel with `devtools::check_win_devel()`
* Update [cran-comments.md](cran-comments.md)
* Commit with `git commit -am "Bump version: x.x.x.9xxx -> x.x.x and re-build
docs."`
* Push with `git push origin dev` and wait for CI builds to pass
* Merge into master:
    ```
    git checkout master
    git merge dev
    git push origin master
    ```
* Tag with `git tag -a vx.x.x`. Summarize [NEWS.md](NEWS.md) entry into bullet
points. Run ` git tag -l -n9` for past examples. Push with `git push origin
--tags`.
* Make a release. On GitHub, go to Releases -> Tags -> Edit release notes. Name
the release "workflowr x.x.x" and copy-paste the Markdown entry from
[NEWS.md](NEWS.md).
* Build tarball with `R CMD build .` and upload to [CRAN submission
site][cran-submit]. You will receive an email to request confirmation, then an
email confirming the package was submitted, and then an email with the test
results. Once it is accepted to CRAN, monitor the [check results][check-results]
for any surpise errors. Also, these builds are when the binaries are built for
Windows and macOS, so they aren't available until they are finished.

[appveyor]: https://ci.appveyor.com
[check-results]: https://cran.r-project.org/web/checks/check_results_workflowr.html
[Codecov]: https://codecov.io/
[contribute]: http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004668#sec011
[covr]: https://github.com/jimhester/covr
[cran-submit]: https://cran.r-project.org/submit.html
[git-tutorial]: http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004668
[hadley]: http://hadley.nz/
[pkgdown]: https://github.com/r-lib/pkgdown
[pt]: https://rstudio.github.io/rstudio-extensions/rstudio_project_templates.html
[r-exts]: https://cran.r-project.org/doc/manuals/R-exts.html
[r-exts-licensing]: https://cran.r-project.org/doc/manuals/R-exts.html#Licensing
[r-pkg]: http://r-pkgs.had.co.nz/
[rhub]: https://r-hub.github.io/rhub/
[roxygen2]: https://github.com/klutometis/roxygen
[style]: http://adv-r.had.co.nz/Style.html
[testthat]: https://github.com/hadley/testthat
[travis]: https://travis-ci.org/
[winbuilder]: https://win-builder.r-project.org/
