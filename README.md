# wcm_io_devops.conga_files

This role provides a generic, reusable way to copy all configuration files generated for a [CONGA](http://devops.wcm.io/conga/) role to the target host. It is meant to be included or declared as dependency by other roles that need to deploy files generated by CONGA.

> This role was developed as part of the
> [wcm.io DevOps Ansible Automation for AEM](http://devops.wcm.io/ansible-aem/)
> to integrate Ansible with
> [CONGA](http://devops.wcm.io/conga/).

## Requirements

This role requires Ansible 2.7 or higher.

## Role Variables

Available variables are listed below, along with default values:

	conga_files_base_path:

The path where the files should be copied to on the target host (required).

	conga_files_directories: "{{ conga_directories }}"

Set of CONGA configuration directories. The role will create these directories as required.

	conga_files_files: "{{ conga_files }}" 

Paths of all configuration files generated by CONGA. The files (including their path) will be copied to the remote host under `conga_files_base_path`.
These two variables default to the corresponding variables set by the [wcm_io_devops.conga_facts](https://github.com/wcm-io-devops/ansible-conga-facts) role (on which this role depends).
	
Additionally, the role expects the following variable to be set by the [wcm_io_devops.conga_facts](https://github.com/wcm-io-devops/ansible-conga-facts) role (on which this role depends):
	
	conga_files_owner:
	conga_files_group:
	conga_files_mode:

Owner, group and mode of the copied files and created directories on the remote host (optional).

	conga_files_handlers:

List of handlers to be notified after the files have been copied (optional).

	conga_files_strip: ''
	conga_files_filter: ''

Optional regular expressions used to modify or filter the list of files to be copied.
The `conga_files_strip` expression is removed from the files on the target host. This is useful if files were generated with an additional path components for organization (e.g. `apache`).
The `conga_files_filter` expression is used to filter the list of files to be copied. Only files matching the expression are processed. This is useful if you don't want to copy all files at once or exclude some altogether.

## Result facts

    conga_files_changed

This fact has the value `true` when the deployment of the conga file(s)
resulted in a change.

## Dependencies

This role depends on the [wcm_io_devops.conga_facts](https://github.com/wcm-io-devops/ansible-conga-facts) role for supplying the list of files and directories to copy.

## Example

Includes the role to copy all files generated in the `apache` directory to  `/etc/httpd` on the target host (without the `apache` prefix) and restarting Apache afterwards.

	 - include_role:
	     name: wcm_io_devops.conga_files
	   vars:
	     conga_files_base_path: "/etc/httpd"
	     conga_files_strip: "^apache/"
	     conga_files_filter: "^apache/"
	     conga_files_handlers: [ restart apache ]

## License

Apache 2.0
