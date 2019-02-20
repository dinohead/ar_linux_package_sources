# linux_package_sources

This role will remove the default distribution package sources and add provided sources on Centos or a Debian-based host. 
Sources can be provided for apt and yum and this role will configure the correct sources based on the detected distribution.

## Requirements

* This role requires root to modify packages sources. <code>ansible_user</code> must be able to sudo to root without a password.
* The role defaults use magic variables. If accepting defaults, facts must be available.

## Role Variables

The variables in this role include:

|parameter|required|default|choices|comments|
|---|---|---|---|---|
|linux_package_sources_apt|false| | |A dictionary containing two keys <code>repo</code> and <code>dest</code>. Both parameters are required. <code>repo</code> Is a string corresponding to a single line in the destination repo file. Refer to https://help.ubuntu.com/community/Repositories/CommandLine for information on source formatting. <code>dest</code> is the path where the file will be written. On Ubuntu this generally should be <code>/etc/apt/sources.list.d/filename.list</code>|
|linux_package_sources_yum|false| | |A dictionary containing the keys <code>name</code>, <code>mirrorlist</code>, <code>baseurl</code>, <code>dest</code>, <code>gpgcheck</code>, <code>gpgkey</code>, <code>sslverify</code>, <code>gpgkey</code>, <code>sslclientcert</code>, <code>sslclientkey</code>. <code>name</code> is the name of the repository to add. This string can not contain spaces. This parameter is required.  <code>mirrorlist</code> and <code>baseurl</code> is the url of the repository to add. Exactly one of these parameters is required. <code>dest</code> is the path where the file will be written. On Centos/RedHat this generally should be <code>/etc/yum.repos.d/filename</code>. This parameter is required. <code>gpgcheck</code> and <code>gpgkey</code> are used to turn on gpg checking for the repository. <code>gpgcheck</code> is a boolean with a default of <code>false</code> and gpgkey is the url of the gpg key. Neither parameter is required. If <code>gpgcheck</code> is <code>true</code>, <code>gpgkey</code> is required. |


## Example Playbook

##### Accept defaults or define sources in inventory
```yaml
    - hosts: servers
      roles:
        - ar_linux_package_sources
```
##### Set apt sources to standard ubuntu and centos repositories
```yaml
    - hosts: servers
      roles:
        - role: ar_linux_package_sources
          linux_package_sources_apt:
            - repo: 'deb [ arch=amd64 ] http://us.archive.ubuntu.com/ubuntu/ {{ ansible_distribution_release }} main restricted universe'
              dest: /etc/apt/sources.list.d/default.list
            - repo: 'deb [ arch=amd64 ] http://us.archive.ubuntu.com/ubuntu/ {{ ansible_distribution_release }}-updates main restricted universe'
              dest: /etc/apt/sources.list.d/default.list
            - repo: 'deb [ arch=amd64 ] http://us.archive.ubuntu.com/ubuntu/ {{ ansible_distribution_release }}-security main restricted universe multiverse'
              dest: /etc/apt/sources.list.d/default.list                           
          linux_package_sources_yum:
            - name: CentOS-7-Base
              mirrorlist: http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
              dest: /etc/yum.repos.d/default
              gpgcheck: true
              gpgkey: https://www.centos.org/keys/RPM-GPG-KEY-CentOS-7       
            - name: CentOS-7-Updates
              mirrorlist: http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
              dest: /etc/yum.repos.d/default
              gpgcheck: true
              gpgkey: https://www.centos.org/keys/RPM-GPG-KEY-CentOS-7            
            - name: CentOS-7-Extras
              mirrorlist: http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
              dest: /etc/yum.repos.d/default
              gpgcheck: true
              gpgkey: https://www.centos.org/keys/RPM-GPG-KEY-CentOS-7
```
## Author Information

|Author              |E-mail                   |
|:-------------------|:------------------------|
|Derek 'dRock' Halsey|derek.halsey@dinohead.com|

## License

MIT License

Copyright (c) 2019 Dinohead LLC

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Role Development Information

### Testing

### Git SCM
Please refer to the .gitignore file and update accordingly depending on your
development environment, etc.  The particular file was generated at 
[gitignore.io](https://www.gitignore.io/) and contains settings for the following:
  - Ansible
  - Python
  - Vim
  - Eclipse
  - IntelliJ IDEA
  - Linux
  - Windows
  
### Versioning
Please update [VERSION.md](./VERSION.md) as you release new versions of your role and try to
abide by [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

### Self-contained
Please try to keep this role as self-contained as possible such that it may be
simply installed (e.g. `ansible-galaxy install`) and applied as part of a 
playbook.
