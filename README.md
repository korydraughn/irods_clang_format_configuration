# irods_clang_format_configuration

This repository contains the .clang-format configuration file used by several iRODS C/C++ projects.

## Projects using this Style Configuration
- irods_capability_indexing
- irods_capability_publishing
- irods_capability_storage_tiering
- irods_client_cli
- irods_client_rest_cpp
- irods_microservice_plugins_curl
- irods_netcdf
- irods_resource_plugin_s3
- irods_rule_engine_plugin_audit_amqp
- irods_rule_engine_plugin_hard_links
- irods_rule_engine_plugin_logical_quotas
- irods_rule_engine_plugin_metadata_guard
- irods_rule_engine_plugin_python

## Adding a Style Configuration to your Project
To add the style configuration file to your project, copy the .clang-format file into the root directory or a parent directory of your project. Clang-Format will check the current directory and then the parent directories until it finds a style configuration file.

## Adding a Git Pre-Commit Hook
Git can run clang-format against your changes before being committed. This guarantees that the formatting of your code always matches the requirements of the project. A pre-commit hook is provided by this repository.

Before you can use the pre-commit hook, you'll need to make sure your git configuration defines the correct properties. To do that, run the following commands:
```bash
git config --global clangformat.binary clang-format
git config --global clangformat.style file
git config --global clangformat.extensions 'h,c,hpp,cpp'
```

To enable the pre-commit hook, do the following:
```bash
cd root/of/your/repo
cd .git/hooks
cp /path/to/irods_clang_format_configuration/pre-commit.template pre-commit
chmod +x pre-commit
```

To test, modify a C/C++ file and commit the changes. You should receive a message stating that you need to format your changes. Instructions for doing so will be provided. Once done, commit your changes and you're all set. The tool will only format the lines modified.

## Notes
- If you change the configuration, make sure to revert any formatting previously done. Not doing so could result in unexpected results.
- Use `git clang-format <branch-or-sha-to-start-from>` instead of `clang-format` so that only the modified files will be formatted.

## Official Clang-Format 13 Documentation
- [Home](http://releases.llvm.org/13.0.0/tools/clang/docs/ClangFormat.html)
- [Style Options](http://releases.llvm.org/13.0.0/tools/clang/docs/ClangFormatStyleOptions.html)
