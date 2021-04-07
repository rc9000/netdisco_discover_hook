# Netdisco discover_hook

This is a Netdisco hook that can suppress the is_uplink detection of interfaces, based on attributes in device_port. It's tied deeply enough into Netdisco to use database parameters and settings from `deployment.yml`.

See https://github.com/netdisco/netdisco/wiki/Hooks for more information.

To run the hook after a device is discovered, put this into deployment.yml

    hooks:
      - type: 'exec'
        event: 'discover'
        with:
          cmd: '/home/netdisco/perl5/bin/discover_hook [% ip %]'

To configure the interfaces that should not be uplinks, add a section like this in deployment.yml. The supplied fields are matched as regex against the columns in device_port,  ie. the second example will turn into `update device_port set is_uplink = false where remote_type ~* 'proliant' and remote_port ~* 'ALOM' and ip = 'discovered_device_ip'`. Any number of columns from device_port can be added. 

    discover_hook:
        no_uplink_port:
            # HP blades typically host a bunch of VMs which we want to show up as nodes
            - remote_type: HP VC FlexFabric
            # Typical node that happens to speak some LLDP and is thus wrongly considered an undiscovered neighbor device
            - remote_type: proliant
              remote_port: ALOM
