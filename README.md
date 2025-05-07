<!-- regenerate: off (set to off if you edit this file) -->

# MBONED: DORMS cluster

This repo is for the MBONED's working group Internet Drafts for DORMS, CBACC, and AMBI.

 * AMBI: Asymmetric Manifest Based Integrity
 * CBACC: Circuit Breaker Assisted Congestion Control
 * DORMS: Discovery of Restconf Metadata for SSM

## Asymmetric Manifest Based Integrity

* [Editor's Copy](https://GrumpyOldTroll.github.io/ietf-dorms-cluster/#go.draft-ietf-mboned-ambi.html)
* [Datatracker Page](https://datatracker.ietf.org/doc/draft-ietf-mboned-ambi)
* [Working Group Draft](https://datatracker.ietf.org/doc/html/draft-ietf-mboned-ambi)
* [Compare Editor's Copy to Working Group Draft](https://GrumpyOldTroll.github.io/ietf-dorms-cluster/#go.draft-ietf-mboned-ambi.diff)

## Circuit Breaker Assisted Congestion Control

* [Editor's Copy](https://GrumpyOldTroll.github.io/ietf-dorms-cluster/#go.draft-ietf-mboned-cbacc.html)
* [Datatracker Page](https://datatracker.ietf.org/doc/draft-ietf-mboned-cbacc)
* [Working Group Draft](https://datatracker.ietf.org/doc/html/draft-ietf-mboned-cbacc)
* [Compare Editor's Copy to Working Group Draft](https://GrumpyOldTroll.github.io/ietf-dorms-cluster/#go.draft-ietf-mboned-cbacc.diff)

## Discovery Of Restconf Metadata for Source-specific multicast

* [Editor's Copy](https://GrumpyOldTroll.github.io/ietf-dorms-cluster/#go.draft-ietf-mboned-dorms.html)
* [Datatracker Page](https://datatracker.ietf.org/doc/draft-ietf-mboned-dorms)
* [Working Group Draft](https://datatracker.ietf.org/doc/html/draft-ietf-mboned-dorms)
* [Compare Editor's Copy to Working Group Draft](https://GrumpyOldTroll.github.io/ietf-dorms-cluster/#go.draft-ietf-mboned-dorms.diff)


## Contributing

See the
[guidelines for contributions](https://github.com/GrumpyOldTroll/ietf-dorms-cluster/blob/master/CONTRIBUTING.md).

Contributions can be made by creating pull requests.
The GitHub interface supports creating pull requests using the Edit (✏) button.


# Building the drafts

The build system for these drafts currently uses [a fork of i-d-template](https://github.com/GrumpyOldTroll/i-d-template/tree/yang-support) derived from the [upstream version](https://github.com/martinthomson/i-d-template), but adding some functionality intended to make it easier to work with [YANG](https://tools.ietf.org/html/rfc7950) models and their examples. (There was a [pull request](https://github.com/martinthomson/i-d-template/pull/193) submitted, but the comments have not yet been addressed and code re-submitted.)

During the make, additional features validate examples against the YANG schema, and produce warning and error messages for YANG models that do not pass the yanglint checks.

In addition to [xml2rfc](https://github.com/martinthomson/i-d-template/blob/main/doc/SETUP.md#xml2rfc) and [kramdown](https://github.com/martinthomson/i-d-template/blob/main/doc/SETUP.md#kramdown-rfc2629), these drafts require pyang and [libyang](https://github.com/CESNET/libyang) (including yanglint):

~~~
python3 -m venv ~/venv-drafts
source ~/venv-drafts/bin/activate
python -m pip install xml2rfc pyang
~~~
