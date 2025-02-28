.. current developments
2022-03-14   3.3.1:
===================

Bug fixes:
----------

* Fix building examples in CI. (#502, #504, #505)

* Fix truncated Python version if minor has two digits on Windows,
  e.g. "3.10". (#506)



2022-03-12   3.3.0:
===================

Enhancements:
-------------

* Initialize mamba (if it exists), too. (#462)
* Add support for Python 3.9 and 3.10. (#479)
* Add an example that uses shortcuts. (#481)
* Expose the installer metadata to pre/post install scripts
  as environment variables ``INSTALLER_NAME``, ``INSTALLER_VER``
  and ``INSTALLER_PLAT``. (#491)

Bug fixes:
----------

* Fixes for transmuting packages and generating repodata. (#489)
* Include cache metadata on the first line of the repodata cache. (#490)
* Fix ``constructor.conda_interface`` to handle alpha, beta, rc versions. (#496)

Deprecations:
-------------

* Drop support for Python 2.7 and 3.6. (#479)

Other:
------

* CI: Run examples outside conda build to upload installers as artifacts for local testing (#498)
* Added project board, issue staleness, thread locking and label automation
  using GitHub action workflows to improve maintenance of GitHub project.

  More information can be found in the infra repo: https://github.com/conda/infra



2022-01-02   3.2.2:
===================
  * Common:
    - Fix crashes due to pyyaml >= 6 deprecating automatic use of SafeLoader; it is now safe to run constructor with pyyaml >= 6 #473

  * Shell:
    - Extract pre-conda before extracting conda packages #450
    - Add tests for header template preprocessing and fix initialize by default #459

  * PKG:
    - Unset DYLD_FALLBACK_LIBRARY_PATH in header on macOS; installer now works with bash >= 5.1.4 #472

  * NSIS:
    - Use nsExec:Exec to remove files and folders instead of using a python subprocess, which fails when removing files still being used #467
    - Add option to disable creation of start menu shortcuts and generally fix shortcut creation #455, #466


2020-03-30   3.2.1:
===================
  * Common:
    - Fixed bug in platform selector regex parsing that was incompatible with using jinja-templated env vars and platform selectors on the same line #428
    - New option in construct.yaml (`batch_mode`) to make passing the `-b` flag the default option #440
    - New option in construct.yaml (`check_path_spaces`) to suppress the "Destination Folder contains spaces" warning on Windows #431
    - Duplicate package files found at environment creation during build on Windows now raises a warning rather than crashing #435

  * Shell:
    - aarch64 installer now properly checks if system is aarch64 #441

  * PKG:
    - Initialize all shells post-install on OSX #444

  * NSIS:
    - change the default prefix for domain user to `%LOCALAPPDATA%` and the option `default_prefix_domain_user` to set its default value #415
    - Fix default installation folder all users and add option `default_prefix_all_users` to set its default value #419
    - Partial fix for removing menu shortcut of other distributions when uninstalling #420
    - Fix a typo in informational message #424
    - Support for custom `nsis_templates` through the `nsis_template` variable. #423


2020-11-14   3.2.0:
===================
  * COMMON:
    - `construct.yaml` file now reads environment variables during the Jinja2 parsing stage. Env vars can be included like: `{{ environ["ENV_VAR"] }}` (#413).
    - Fixed bug where constructor crashed if a listed spec was in a noarch-only channel (i.e. without a subdir for the specific platform) (#409).
    - Avoid adding newly installed environment to the list of global conda environments if conda isn't included in the installer itself. (#372)

  * SHELL:
    - The user-shell initialisation phase of the installer is no longer included in the install script if there is no conda available in the installed environment.
    - The "test" option is only available if conda is installed in the environment. Previously this would fail in this situation.

2020-09-31   3.1.0:
===================
  * COMMON:
    - New platform selectors: s390x, osx-arm64
    - Added the ability to build installers from an existing,
      instantiated environment or an environment.yml spec.
    - Added conda_channel_alias and condarc options to allow
      more complete customization of the installed .condarc
    - Added options to all install types to preserve the
      package cache after installation
    - Migrated CI to GitHub Actions and added installation/
      unpack testing of generated installers

  * SHELL:
    - Pre- and post-install scripts are now executed directly
      if a shebang is present; otherwise they are run by the
      POSIX shell `sh`.

  * NSIS:
    - Support for pre-install script
    - Added the ability to check if the path length is longer
      than 46 characters, so that nodejs package contents will
      not exceed a path length of 260 characters.

  * PKG:
    - Added an "all" installer type option to support building
      both .pkg and .sh installers with a single commmand

2019-09-16   3.0.1:
===================
  * COMMON:
    - Add dependency on standalone conda executable

2019-08-09   3.0.0:
===================
  * COMMON:
    - this utilizes a fundamentally different approach from before.  A standalone
      conda executable (created with pyinstaller) is used to create environments
      from packages that are shipped with the installers. This allows full support
      for everything that is supported by the standalone conda.  It does add some
      complexity in terms of menu creation and other operations that rely on
      knowing whether the env in use (the temporary env for the standalone exe) is
      the base env.  Let's call those prefix-based operations.

2019-11-04   2.3.0:
===================
  * COMMON:
    - Drop redundant code in install.py
    - Fix bug in size computation for pkgs built with older conda build

  * NSIS:
    - Allow configuring the uninstaller name
    - Always pass absolute path to makensis

  * SHELL:
    - Add new line to bash config before modifying it

2018-09-30   2.2.0:
===================
  * COMMON:
    - Refactor constructor internals to use conda 4.5.x APIs
    - Create and add <pkg>/info/repodata_record.json for each $pkg to preconda
    - Use 'conda init' equivalent for modifying bashrc/bash_profile
    - Rename parameter `add_to_path_default` to `initialize_by_default`

  * NSIS:
    - Replace hardcoded 'Anaconda' with ${NAME}
    - Disallow ',' character in the installation path
    - Check if $INSTDIR is writable before proceeding to install

  * PKG:
    - Fix wording in installer README


2018-07-14   2.1.1:
===================
  * NSIS:
    - Don't fail when index cache is empty (local channels)

  * SHELL:
    - Fix wording for force/update on an existing installation


2018-06-06   2.1.0:
===================
  * COMMON:
    - Populate conda-meta/history file properly
    - Remove references to free channel
    - Replace references to continuum.io with anaconda.com
    - Officially drop support for 'packages' key
    - Fix compatibility with newer version of ruamel.yaml

  * NSIS:
    - Disallow ^, %, ! and = characters in the installation path
    - Restrict PATH env variable to a minimal required set

  * PKG:
    - Add fix for marking .app bundles non relocatable


2018-04-03   2.0.3:
===================
  * COMMON:
    - Fix incompatibility with conda 4.5.x
    - Decouple doc generation from source code, templatize it


2018-02-01   2.0.2:
===================
  * COMMON:
    - Document platform selectors and new keys
    - Drop usage of libconda
    - Improve compatibility with Conda 4.4
    - Don't assume that attempt_hardlinks is always defined
    - Fix double use of info as different types

  * NSIS:
    - Append suffix to GetTempFileName() before using it

  * PKG:
    - Change ownership back to $USER after updating dot profile

  * SHELL:
    - Add more os, arch checks to the shell installer

2017-11-16   2.0.1:
===================
  * COMMON:
    - Add CI testing for Travis and Appveyor
    - Write basic system info to $PREFIX\pkgs\.constructor-build.info
    - Ignore folders in tarballs while checking for duplicates
    - Ensure approx_pkgs_size_kb is an integer
    - Fix broken tests


2017-11-01   2.0.0:
===================
  * COMMON:
    - Add support for channel remapping
    - Make sure $PREFIX/envs is created by the installers
    - Fallback to cat if more is not available
    - Allow company name to be specified in construct.yaml
    - Add feature to check file duplicates across dists
    - Switch requirements to conda, ruamel_yaml
    - Add write_condarc option
    - Don't assume that channel keys will always be available
    - Parameterize installer name at various locations
    - Add support for pre-populating repodata cache
    - Introduce 'attempt_hardlinks' option
    - Copy pkgs to conda-bld (local) channel and test from there
    - Also add channels to .condarc
    - Remove urljoin import
    - Fix bug when downloading packages
    - Prefer conda via conda_interface instead of libconda
    - Add dry run option
    - Switch to setuptools
    - Remove eval from setup.py, use absolute paths
    - Add customization for welcome and header image texts
    - Add support and examples for aarch64
    - Add basic jinja2 support

  * NSIS:
    - Compute an approx. size for installation
    - Allow more than one vsXXXX runtime, but warn
    - Fix registry key handling
    - disallow installation when any files present in destination folder
    - Fix 'all users/just me' installation handling
    - Parameterize installation location for all users
    - Improve spaces/non-ascii/unicode character handling in nsis installer
    - Extract python and DLLs to %PREFIX%/%randomdir and ./.install from there
    - Fix several aspects of PATH env var management
    - Fix wording in Windows installer
    - Change AddToPath to not be the default
    - Add support for command line installation for Windows
    - Use ctypes for creating hard links on win
    - NSIS: Copy index cache directory
    - Fix issue when using conda to solve on windows
    - Add ability to make nsis verbose
    - Remove menus of all conda envs during uninstall
    - Add ability to provide defaults for custom options

  * PKG:
    - Add support for signing the pkg installer
    - Flip enable_{anywhere,localSystem}

  * SHELL:
    - Warn user if PYTHONPATH env var is set
    - Handle spaces in path to be patched
    - Compress non tarball files into preconda.tar.bz2
    - Standardise header.sh redirects
    - Add -t option to test the installer
    - Use getopt if available, fallback to getopts
    - Add more tests for RUNNING_SHELL
    - Remove bashisms from header.sh, using only POSIX, split tar and bunzip2


2017-08-XX   1.7.0:
===================
  * add support for creating .pkg installers on OSX, #98


2017-??-??   1.6.0:
===================
  * ???


2017-03-30   1.5.5:
===================
  * proved access to LD_LIBRARY_PATH in Linux install scripts by storing it
    as OLD_LD_LIBRARY_PATH
  * replace '//' by '/' for install.py --root-prefix option
  * turn error about wrong menu_packages into warning
  * add warning to shell installers when bzip2 is not executable


2017-02-16   1.5.4:
===================
  * skip binary prefix replacement on Windows, #62
  * add writing empty conda-meta/history upon installation


2017-01-31   1.5.3:
===================
  * update Visual Studio version map to with with Python 3.6 on Windows
  * add unicode line, update version comment, #61
  * add --clean (cache) option


2017-01-12   1.5.2:
===================
  * unlink files prior to writing with a new prefix, #58
  * fix test against NSIS 3.01


2017-01-06   1.5.1:
===================
  * add --cache-dir option, which defaults to CONSTRUCTOR_CACHE when set,
    or ~/.conda/constructor otherwise
  * fix typo


2016-11-07   1.5.0:
===================
  * add -u (update) option to resulting .sh installer, see #46


2016-10-20   1.4.2:
===================
  * allow '-' character in version


2016-10-19   1.4.1:
===================
  * add simple check for valid name and version


2016-10-06   1.4.0:
===================
  * add menu_packages key in construct.yaml


2016-09-15   1.3.4:
===================
  * add -s option to shell installer to run without executing user-defined
    scripts, basically #44
  * allow NSIS 3 to be used to Windows


2016-09-12   1.3.3:
===================
  * add support for 'noarch' packages


2016-08-11   1.3.2:
===================
  * bug: allow '-' in package name, when using 'exlcude' key


2016-07-19   1.3.1:
===================
  * add pkgs/urls.txt to be compatible with current conda
  * add 'md5' and 'installed_by' keys to conda-meta/<dist>.json metadata
    for installed packages


2016-07-08   1.3.0:
===================
  * add ability to run `post-link` scripts (inside conda packages) on Windows
  * add ability to run post install `.bat` scripts on Windows
  * improve install logic on Unix, replace post.py by custom install.py,
    which is independent of conda
  * remove dependency on conda, we now use libconda, which also means that
    constructor can be installed into a non-root environment


2016-06-24   1.2.1:
===================
  * compatibility with conda 4.1, see #26
  * include urls.txt in the pkgs, #27
  * skip machine type check in batch mode (Unix)


2016-04-07   1.2.0:
===================
  * ensure empty lists are handled correctly with selectors
  * add keep_pkgs option to construct.yaml


2016-03-24   1.1.0:
===================
  * add support for pre and post install scripts on Unix
  * fix issues related to non x86 platforms
  * add default_prefix support for Windows, see #7 and #14


2016-03-02   1.0.0:
===================
  * initial release
