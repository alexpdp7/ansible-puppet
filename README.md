# ansible-puppet

A simple role to run Puppet via Ansible, with a hacky (and unsafe) way to use secrets from vaults (or regular vars) in your Puppet manifests

## Motivation
 
* To play with [git subrepo](https://github.com/ingydotnet/git-subrepo) to publish parts of my private monorepo as public repos
* I prefer Puppet for configuration management, Ansible for orchestration. I also like Ansible vaults for handling secrets.

## Usage
 
I use the following directory structure:
 
* `ansible-puppet.yml`:

        ---
        - hosts: all
          roles:
            - ansible-puppet
* `puppet`
  * `site`
     * `00-common.pp`
     * `node1.pp`
     * ...
  * `modules`

Running `ansible-playbook ansible-puppet.yml` will apply `site` on all Ansible hosts.

If you have `host_vars/vars.yml`:

    ---
    puppet_env:
      FACTER_SECRET_VAR: "{{ vault_FACTER_SECRET_VAR }}"
      FACTER_PUBLIC_VAR: public_var

and the corresponding vault variable defined, you can simply do `$public_var` and `$secret_var` in your manifests and it will work.

NOTE: this works by pushing environment variables and having Puppet access them via facter, which is not safe.
