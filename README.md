Tutorials
=========

This README is for contributors.  The tutorials are publicly available at [https://eecs280staff.github.io/tutorials/](https://eecs280staff.github.io/tutorials/).

EECS 280 C/C++ tools and tutorials.


## Markup source
The markup source files live in `docs/`.  We're using GitHub pages to publish the `docs/` folder using the [Primer Spec custom theme](https://github.com/eecs485staff/primer-spec).

### Local preview
For local spec development, render GitHub pages locally using the [Primer Spec instructions](https://github.com/eecs485staff/primer-spec/blob/develop/docs/USAGE_ADVANCED.md).  Summary:

One time install:
```console
$ gem install bundler
$ bundle install
```

Every day usage:
```console
$ cd docs/
$ bundle exec jekyll serve
```

Troubleshooting: Remove the lockfile and try again.  It's not committed to avoid problems with GitHub Pages deploy.
```console
$ rm Gemfile.lock
```

### Pull request spec previews
We generate previews of the spec website whenever a Pull Request modifies the `docs/` directory. These previews are uploaded to `https://preview.seshrs.ml/previews/eecs280staff/<repo-name>/<PR-number>/` using a marketplace GitHub Action ([`upload-to-primer-spec-preview`](https://github.com/seshrs/upload-to-primer-spec-preview)). Check out the [workflow definition](.github/workflows/spec_preview.yml) for implementation details.

## License
The documents in this repository are licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with the documents in this repository.
