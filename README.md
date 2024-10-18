# Ansible Collection - get_edi_io.debian_setup

This Ansible collection contains roles that extend and complement the
[edi tool](https://docs.get-edi.io/en/stable/introduction.html).

## apt_cleanup

In order to reduce the footprint of images this role removes apt cache items that can be re-downloaded using
`apt update`. Furthermore, it removes the `/usr/sbin/policy-rc.d` file that got used during the build process.

## apt_setup

Tweaks the apt setup to get small target system images.

### Parameters

 * *install_documentation:*
     >By default (value `full`) the documentation of every Debian package will get installed.
     Switch this value to :code:`minimal` if you want to deploy an image with a minimal footprint.
     Switch this value to :code:`changelog` if you want to minimize the footprint but keep the changelog of all
     packages.
 * *translations_filter:*
     >By default all translations contained in Debian packages will get installed (empty filter: `""`).
     To reduce the footprint of the resulting artifacts the number of installed languages can be limited.
     By choosing the builtin filter `"en_translations_only"` you can make sure that only English
     translations will get installed.
* *base_system_sources_list_template:*
     >During bootstrapping a minimal `/etc/apt/sources.list` file gets added to the root file system. By specifying
     a template using `base_system_sources_list_template` the initial sources list file will get removed and
     replaced by a file containing the content of the rendered template. The new file will be written to the
     `/etc/apt/sources.list.d` subfolder. `edi` provides the template `debian.list` that can be chosen
     as a good starting point. In case `debian.list` does not meet the requirements a custom template
     can be specified. By default, no template gets applied.

## default_user

Adds a default user to the target setup.

### Parameters

* *default_user_group_name:*
     >The group name of the default user (default is `edi`).
* *default_user_gid:*
     >The group id of the default user (default is `2000`).
* *default_user_name:*
     >The user name of the default user (default is `edi`).
* *default_user_uid:*
     >The user id of the default user (default is `2000`).
* *default_user_shell:*
     >The shell of the default user (default is `/bin/bash`).
* *default_user_groups:*
     >The groups of the default user (default is `adm,sudo`).
* *default_user_password:*
     >The initially set password of the default user
     (default is `ChangeMe!`). You can
     [adjust this password](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module)
     if needed.
     Set this password to `"*"` if
     you would like to disable password based login. Please note that
     the playbook will then automatically create a sudoers file to not
     impair the `sudo` command.
* *authorize_current_user:*
     >By default (boolean value `True`) the current host user will be authorized to ssh into the account
     of the default user. Switch this value to `False` if the current user shall not be authorized.
* *ssh_pub_key_directory:*
     >All the public keys (ending with .pub) contained in the folder `ssh_pub_key_directory` (defaults to
     `{{ edi_project_directory }}/ssh_pub_keys`) will be added to the list of authorized ssh keys of the
     default user.

## development_tools

Adds development tools to the target setup.

## document_artifact

Adds documentation to the target setup.

### Parameters

* *document_build_setup:*
     >To document the build setup of the artifact within the artifact set this value to `True`.
     As a result the file `/usr/share/doc/edi/build.yml` will be generated. By default, this feature is switched
     off (boolean value `False`).
* *document_installed_packages:*
     >To document the packages of the artifact within the artifact set this value to `True`.
     As a result the file `/usr/share/doc/edi/packages.yml` will be generated. The generated file will contain a
     list of all packages including version information. It is a snapshot of the available packages after the artifact
     build and will not get updated when new packages get installed using `dpkg` or `apt`.
     By default this feature is switched off (boolean value `False`).
* *package_baseline_source_file:*
     >In order to generate a differential changelog it is possible to add a package baseline file to the resulting
     artifact. The package baseline file has the same format as `/usr/share/doc/edi/packages.yml`. If a differential
     changelog between release n and n+1 is needed, you can copy the file `/usr/share/doc/edi/packages.yml` from
     release n to `{{ edi_project_directory }}/configuration/documentation/packages-baseline.yml` (default value
     for package_baseline_source_file). The playbook will then make sure that it gets added to artifact n as
     `/usr/share/doc/edi/packages-baseline.yml`. The command `edi documentation render ...` will use this
     information to restrict the changelog to changes that happened between release n and n+1.

## gitops_tools

Adds gitops tools to the target setup.

## gitops_user

Adds a gitops user to the target setup.

## multiarch_support

Enables foreign architectures on target setup.

## openssh_server

Installs the openssh server on the target setup.

### Parameters

* *disable_ssh_password_authentication:*
     >By default, password authentication is disabled for ssh (boolean value `True`). If you want to
     allow password based authentication then switch this value to `False` but make sure to use a non-standard
     password.

## openssh_server_keys

Makes sure that each target setup gets unique ssh server keys.

### Parameters

* *ssh_host_key_backup_folder:*
     >Optionally the ssh host keys can be restored from a folder on the first boot. The restore script will not
     overwrite existing valid keys in `/etc/ssh/`. By default
     this feature is turned off (ssh_host_key_backup_folder is `""`). This feature is especially useful
     if ssh host keys shall be preserved during a complete OS update.

## proxy_settings

This role can be used to inject proxy settings during the build process. Furthermore, the role can also be used
to clean up the proxy settings at the end of the build process.

### Parameters

* *target_http_proxy:*
     >The http proxy that gets applied to the target system (defaults to `{{ edi_host_http_proxy }}`).
* *target_https_proxy:*
     >The https proxy that gets applied to the target system (defaults to `{{ edi_host_https_proxy }}`).
* *target_ftp_proxy:*
     >The ftp proxy that gets applied to the target system (defaults to `{{ edi_host_ftp_proxy }}`).
* *target_socks_proxy:*
     >The socks proxy that gets applied to the target system (defaults to `{{ edi_host_socks_proxy }}`).
* *target_no_proxy:*
     >The proxy exception list that gets applied to the target system
     (defaults to `{{ edi_host_no_proxy }}`).

## replace_temp_hostname

Replaces the target setup host name at the end of the build process.

* *hostname:*
     >Set the hostname within the final artifact (default is `edi`).
