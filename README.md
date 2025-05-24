# Ansible Role: wslconfig

Ansible role to manage Windows Subsystem for Linux (WSL) global configuration file [`.wslconfig`](https://github.com/greengorych/wsl-configurations/tree/main/defaults/.wslconfig).

By default, this role generates and manages a `.wslconfig` file that includes:
- Variable descriptions
- Variable dependencies
- Default values
- Possible values
- Variables with default or overridden values

## Requirements

- Requires Windows 10 version 19041 (20H1) or newer for most `.wslconfig` settings.
- Windows Subsystem for Linux (WSL) must already be installed and configured on the target system.
- The user running Ansible must have write access to the `.wslconfig` file in the Windows user profile directory (`C:\Users\<UserName>\.wslconfig`).
- Ansible version 2.12 or higher is recommended.

## Role Variables

The following variables allow customization of the `.wslconfig` file and its management by this role.

> [!IMPORTANT]
> - You can set or override the default role variables in your playbook or in [`vars/main.yml`](https://github.com/greengorych/ansible-role-wslconfig/blob/main/vars/main.yml).
> - By default, all variable values are equivalent to the default behavior when no `.wslconfig` file is present.

### General variables

| Variable        | Default                                       | Description                          |
| --------------- | --------------------------------------------- | ------------------------------------ |
| wslconfig_owner | {{ ansible_user_id }}                         | File owner (Windows username)        |
| wslconfig_group | {{ ansible_user_id }}                         | File group (same as user by default) |
| wslconfig_path  | /mnt/c/Users/{{ wslconfig_owner }}/.wslconfig | Full path to `.wslconfig` file       |

> [!IMPORTANT]
> - The `wslconfig_owner` and `wslconfig_group` role variables defaults to `{{ ansible_user_id }}`, resolving to the user running the playbook. This ensures the default WSL user matches the Ansible connection user.
> - The `wslconfig_path` role variable defaults uses `{{ wslconfig_owner }}` to determine the path to `.wslconfig`.
> - Ensure `gather_facts: true` is enabled in your playbook, or manually define the `wslconfig_owner`, `wslconfig_group` and `wslconfig_path` variable in your playbook or in `vars/main.yml`.

### Mapping Role Variables to .wslconfig Settings

| Role variable                                     | Configuration section | Configuration variable  |
| ------------------------------------------------- | --------------------- | ----------------------- |
| wslconfig_wsl2_kernel                             | [wsl2]                | kernel                  |
| wslconfig_wsl2_kernel_modules                     | [wsl2]                | kernelModules           |
| wslconfig_wsl2_kernel_cmdline                     | [wsl2]                | kernelCommandLine       |
| wslconfig_wsl2_processors                         | [wsl2]                | processors              |
| wslconfig_wsl2_memory                             | [wsl2]                | memory                  |
| wslconfig_wsl2_swap                               | [wsl2]                | swap                    |
| wslconfig_wsl2_swap_file                          | [wsl2]                | swapFile                |
| wslconfig_wsl2_default_vhd_size                   | [wsl2]                | defaultVhdSize          |
| wslconfig_wsl2_vm_switch                          | [wsl2]                | vmSwitch                |
| wslconfig_wsl2_networking_mode                    | [wsl2]                | networkingMode          |
| wslconfig_wsl2_mac_address                        | [wsl2]                | macAddress              |
| wslconfig_wsl2_dhcp                               | [wsl2]                | dhcp                    |
| wslconfig_wsl2_ipv6                               | [wsl2]                | ipv6                    |
| wslconfig_wsl2_localhost_forwarding               | [wsl2]                | localhostForwarding     |
| wslconfig_wsl2_dns_proxy                          | [wsl2]                | dnsProxy                |
| wslconfig_wsl2_dns_tunneling                      | [wsl2]                | dnsTunneling            |
| wslconfig_wsl2_firewall                           | [wsl2]                | firewall                |
| wslconfig_wsl2_auto_proxy                         | [wsl2]                | autoProxy               |
| wslconfig_wsl2_gui_applications                   | [wsl2]                | guiApplications         |
| wslconfig_wsl2_nested_virtualization              | [wsl2]                | nestedVirtualization    |
| wslconfig_wsl2_vm_idle_timeout                    | [wsl2]                | vmIdleTimeout           |
| wslconfig_wsl2_safe_mode                          | [wsl2]                | safeMode                |
| wslconfig_wsl2_debug_console                      | [wsl2]                | debugConsole            |
| wslconfig_experimental_auto_memory_reclaim        | [experimental]        | autoMemoryReclaim       |
| wslconfig_experimental_sparse_vhd                 | [experimental]        | sparseVhd               |
| wslconfig_experimental_best_effort_dns_parsing    | [experimental]        | bestEffortDnsParsing    |
| wslconfig_experimental_dns_tunneling_ip_address   | [experimental]        | dnsTunnelingIpAddress   |
| wslconfig_experimental_initial_auto_proxy_timeout | [experimental]        | initialAutoProxyTimeout |
| wslconfig_experimental_ignored_ports              | [experimental]        | ignoredPorts            |
| wslconfig_experimental_host_address_loopback      | [experimental]        | hostAddressLoopback     |

### [wsl2] section variables

| Variable            | Values                               | Default | Description                                |
| --------------------| ------------------------------------ | ------- | ------------------------------------------ |
| kernel              | Path to kernel image                 | Not set | Custom Linux kernel path                   |
| kernelModules       | Path to modules VHD                  | Not set | Path to VHD with kernel modules            |
| kernelCommandLine   | String                               | Not set | Extra kernel boot parameters               |
| processors          | Integer                              | Not set | Number of logical processors to use        |
| memory              | Integer (MB or GB)                   | Not set | Limit RAM usage                            |
| swap                | Integer (0 disables swap)            | Not set | Size of swap file                          |
| swapFile            | Path                                 | Not set | Custom path to swap file                   |
| defaultVhdSize      | Size in GB or TB                     | 1TB     | Maximum size of the root VHD file          |
| vmSwitch            | String                               | Not set | Name of virtual switch (bridged mode)      |
| networkingMode      | nat, mirrored, bridged, virtioproxy  | nat     | Network mode to use                        |
| macAddress          | MAC address string                   | Not set | Fixed MAC address                          |
| dhcp                | true, false                          | true    | Enable DHCP support                        |
| ipv6                | true, false                          | true    | Enable IPv6 support                        |
| localhostForwarding | true, false                          | true    | Allow localhost port forwarding            |
| dnsProxy            | true, false                          | true    | Proxy DNS through Windows service          |
| dnsTunneling        | true, false                          | true    | Use Windows networking stack for DNS       |
| firewall            | true, false                          | true    | Use Windows Firewall and Hyper-V filters   |
| autoProxy           | true, false                          | true    | Automatically use Windows proxy settings   |
| guiApplications     | true, false                          | true    | Enable GUI support (WSLg)                  |
| nestedVirtualization| true, false                          | true    | Enable nested virtualization               |
| vmIdleTimeout       | Integer                              | 60000   | Shutdown timeout after idle (ms)           |
| safeMode            | true, false                          | false   | Enable safe recovery mode                  |
| debugConsole        | true, false                          | false   | Show debug console for `dmesg` output      |

### [experimental] section variables

| Variable                | Values                       | Default        | Description                                   |
| ------------------------| ---------------------------- | -------------- | --------------------------------------------- |
| autoMemoryReclaim       | disabled, gradual, dropcache | disabled       | Strategy for reclaiming unused memory         |
| sparseVhd               | true, false                  | true           | Use sparse VHDs to save space                 |
| bestEffortDnsParsing    | true, false                  | false          | Tolerate partial DNS config parsing           |
| dnsTunnelingIpAddress   | IP address                   | 10.255.255.254 | Address to use for DNS tunneling              |
| initialAutoProxyTimeout | Integer (ms)                 | 10000          | Timeout for applying auto proxy settings      |
| ignoredPorts            | List of integers or ranges   | Not set        | List of ports to exclude from WSL forwarding  |
| hostAddressLoopback     | true, false                  | false          | Loopback behavior for host address inside WSL |

## Dependencies

This role has no external Ansible dependencies.

## Example Playbook

```yaml
- name: Managing .wslconfig
  hosts: localhost
  gather_facts: true
  roles:
    - { role: ansible-role-wslconfig }
```

## License

MIT - free to use, modify, and share.

## Author Information

This role was created by [greengorych](https://github.com/greengorych).

## Related Links

- [.wslconfig – Default WSL2 Configuration File](https://github.com/greengorych/wsl-configurations/tree/main/defaults/.wslconfig)
