# Netdisco discover_hook

This is a Netdisco hooks that can suppress the is_uplink detection of interfaces, based on attributes in device_port.
See https://github.com/netdisco/netdisco/wiki/Hooks for more information.

To run the hook after a device is discovered, put this into deployment.yml

    hooks:
      - type: 'exec'
        event: 'discover'
        with:
          cmd: '/home/netdisco/perl5/bin/discover_hook [% ip %]'

To configure the interfaces that should not be uplink, add a section like this in deployment.yml. the supplied fields are matched as regex against the columns in device_port,  ie. "update device_port set is_uplink = false where remote_type ~* '<val>' and remote_port ~* '<val>' and ip = 'hook_ip'. Any number of columns from device_port can be added. 

    discover_hook:
        no_uplink_port:
            # HP blades typically host a bunch of VMs which we want to show up as nodes
            - remote_type: HP VC FlexFabric
            # Typical node that happens to speak some LLDP and is thus wrongly considered an undiscovered neighbor device
            - remote_type: proliant
              remote_port: ALOM
