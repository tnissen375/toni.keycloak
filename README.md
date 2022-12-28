Role Name
=========

Ansible role to install keycloak open source identity and access management which automaticly integrates with an (authenticating) openresty reverse-proxy. Its used by me to run SSO stacks. It uses a preconfigured realm called "Intern" and a client called "Nginx" if you dont configure it otherwise. You have to change the **keycloak_client_secret** before you install for security reasons.


Some variables from the role default section wich can be overriden if needed.
```yaml
deploy_name: "key" # will be used to prefix docker stack
nginx_subdomain: "{{ deploy_name }}." # subdomain derived from deploy name, you do not need to change it - but you can
nginx_upstream: "http://{{ deploy_name }}:8080" # nginx stack needs to know where to reach keycloak, you do not need to change it - but you can
dns_create: false # only if you use hetzner hosting provider or extended the toni.dns role to take other providers - so dont use
dns_update: false # only if you use hetzner hosting provider or extended the toni.dns role to take other providers - so dont use
nginx_stack: "openresty" # ansible needs to know the name you gave the openresty stack
nginx_stack_keep_alive: false # during setup its possible to restart the openresty stack or to reload it (keep alive)
nginx_network_name: "nginx_net" # default - not created by this stack
create_stack_network: true # default - seperate networks for each stack
stack_network_name: "key_net" # default - will be created for this stack
stack_subnet: "10.10.20.0/24" # :)

keycloak_image: "quay.io/keycloak/keycloak:20.0.2" # keycloak image used
keycloak_pg_image: "postgres:14" # postgres image used

keycloak_realm: "Intern"
keycloak_client_name: "Nginx"
keycloak_client_secret: "d8337b2d-85fb-46c0-9fcb-15e2dfeb4d9b" # IMPORTANT: SET YOUR OWN SECRET - notation as to the left
keycloak_redirect: "https://lab.<your_domain>.com/" #adjust

# Configure your email here, so that keycloak can send you mails
# if using gmail account enable "insecure" apps in you mail account 
keycloak_server: "{{ nginx_subdomain }}{{ nginx_domain }}"
keycloak_smtp_password: ""
keycloak_smtp_tls: "true"
keycloak_smtp_auth: "true"
keycloak_smtp_port: "587" # p.e.
keycloak_smtp_host: "smtp.<your_domain>.com" # adjust
keycloak_smtp_from: ""
keycloak_smtp_display_name: ""
keycloak_smtp_ssl: "false"
keycloak_smtp_user: ""
```

Fill in your <domain>. A realm "Intern" will be created. A client named Nginx will be created.
```yaml
ansible-playbook install.yml -i ../toni.ansible_inventories/server --vault-id corteza@vault --tags "keycloak, kc_realm" --skip-tags "kc_role" --extra-vars "ansible_ssh_host=89.58.8.242 deploy_name=key nginx_domain=<domain> keycloak_client_name=Nginx"
```

License
-------

MIT