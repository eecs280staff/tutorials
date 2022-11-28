Tutorials
=========

This README for contributors.  The tutorials are publicly available at [https://eecs280staff.github.io/tutorials/](https://eecs280staff.github.io/tutorials/).

EECS 280 C/C++ tools and tutorials.


## Markup source
The markup source files live in `docs/`.  We're using GitHub pages to publish the `docs/` folder using the [Primer Spec custom theme](https://github.com/eecs485staff/primer-spec).

### Local preview
For local spec development, render GitHub pages locally using the [Primer Spec instructions](https://github.com/eecs485staff/primer-spec/blob/develop/docs/USAGE_ADVANCED.md).  Summary:

One time install:
```console
$ ruby --version  # Need version 2.4.0 or later
ruby 2.6.1p33 (2019-01-30 revision 66950) [x86_64-darwin18]
$ gem install bundler
$ pwd
/Users/awdeorio/src/eecs280/eecs281-setup
$ bundle install
```

Every day usage:
```console
$ pwd
/Users/awdeorio/src/eecs280/eecs281-setup/docs
$ bundle exec jekyll serve
```

If you run into Jekyll errors, run `bundle update`:
```console
$ pwd
/Users/awdeorio/src/eecs280/eecs281-setup
$ bundle update --all
```

If `Gemfile.lock` changes, be sure to commit the changes.

### Pull request spec previews
We generate previews of the spec website whenever a Pull Request modifies the `docs/` directory. These previews are uploaded to `https://preview.seshrs.ml/previews/eecs280staff/<repo-name>/<PR-number>/` using a marketplace GitHub Action ([`upload-to-primer-spec-preview`](https://github.com/seshrs/upload-to-primer-spec-preview)). Check out the [workflow definition](.github/workflows/spec_preview.yml) for implementation details.

## License
The documents in this repository are licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You’re free to copy and share this document, but not to sell it. You may not share source code provided with the documents in this repository.
