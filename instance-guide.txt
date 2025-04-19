# Создание ВМ Cloud-HA01
resource "openstack_compute_instance_v2" "Cloud_HA01" {
  name            = "Cloud-HA01"
  image_id        = var.image_id
  flavor_id       = var.flavor_id1
  key_pair        = var.key_pair_name
  security_groups = ["default", "icmp"]

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    destination_type      = "volume"
    volume_size           = 10
    boot_index            = 0
    delete_on_termination = true
  }

  network {
    port = openstack_networking_port_v2.ha01_port.id
  }

  # Добавляем второй интерфейс
  network {
    port = openstack_networking_port_v2.ha01_port1.id
  }
 # Добавляем второй интерфейс
  network {
    port = openstack_networking_port_v2.ha01_port2.id
  }
}
resource "openstack_networking_port_v2" "ha01_port" {
  name           = "ha01-port"
  network_id     = openstack_networking_network_v2.external_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.external_subnet.id
    ip_address = "172.16.1.2"
  }
}

resource "openstack_networking_port_v2" "ha01_port1" {
  name           = "ha01-port1"
  network_id     = openstack_networking_network_v2.internal_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.internal_subnet.id
    ip_address = "192.168.10.1"  # Измененный уникальный IP
  }
}
resource "openstack_networking_port_v2" "ha01_port2" {
  name           = "ha01-port2"
  network_id     = openstack_networking_network_v2.management_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.management_subnet.id
    ip_address = "192.168.10.65"  # Измененный уникальный IP
  }
}

# Создание второй ВМ
resource "openstack_compute_instance_v2" "Cloud_HA02" {
  name            = "Cloud-HA02"
  image_id        = var.image_id
  flavor_id       = var.flavor_id1
  key_pair        = var.key_pair_name
  security_groups = ["default", "icmp"]

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    destination_type      = "volume"
    volume_size           = 10
    boot_index            = 0
    delete_on_termination = true
  }

  network {
    port = openstack_networking_port_v2.ha02_port.id
  }

  network {
    port = openstack_networking_port_v2.ha02_port1.id
  }
 network {
    port = openstack_networking_port_v2.ha02_port2.id
  }
}
resource "openstack_networking_port_v2" "ha02_port" {
  name           = "ha02-port"
  network_id     = openstack_networking_network_v2.external_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.external_subnet.id
    ip_address = "172.16.1.3"
  }
}

resource "openstack_networking_port_v2" "ha02_port1" {
  name           = "ha02-port1"
  network_id     = openstack_networking_network_v2.internal_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.internal_subnet.id
    ip_address = "192.168.10.2"  # Измененный уникальный IP
  }
}
resource "openstack_networking_port_v2" "ha02_port2" {
  name           = "ha02-port2"
  network_id     = openstack_networking_network_v2.management_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.management_subnet.id
    ip_address = "192.168.10.66"  # Измененный уникальный IP
  }
}

# Создание ВМ Cloud-WEB01
resource "openstack_compute_instance_v2" "cloud_WEB01" {
  name            = "Cloud-WEB01"
  image_id        = var.image_id
  flavor_id       = var.flavor_id2  # Используем второй шаблон
  key_pair        = var.key_pair_name
  security_groups = ["default", "icmp"]

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    destination_type      = "volume"
    volume_size           = 10
    boot_index            = 0
    delete_on_termination = true
  }

  network {
    port = openstack_networking_port_v2.WEB01_port.id
  }
network {
    port = openstack_networking_port_v2.WEB01_port2.id
  }
}
resource "openstack_networking_port_v2" "WEB01_port" {
  name           = "WEB01-port"
  network_id     = openstack_networking_network_v2.internal_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.internal_subnet.id
    ip_address = "192.168.10.3"  # Уникальный IP
  }
}
resource "openstack_networking_port_v2" "WEB01_port2" {
  name           = "WEB01-port2"
  network_id     = openstack_networking_network_v2.management_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.management_subnet.id
    ip_address = "192.168.10.67"  # Измененный уникальный IP
  }
}

# Создание ВМ Cloud-WEB02
resource "openstack_compute_instance_v2" "cloud_WEB02" {
  name            = "Cloud-WEB02"
  image_id        = var.image_id
  flavor_id       = var.flavor_id2  # Используем второй шаблон
  key_pair        = var.key_pair_name
  security_groups = ["default", "icmp"]

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    destination_type      = "volume"
    volume_size           = 10
    boot_index            = 0
    delete_on_termination = true
  }

  network {
    port = openstack_networking_port_v2.WEB02_port.id
  }
network {
    port = openstack_networking_port_v2.WEB02_port2.id
  }
}

resource "openstack_networking_port_v2" "WEB02_port" {
  name           = "WEB02-port"
  network_id     = openstack_networking_network_v2.internal_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.internal_subnet.id
    ip_address = "192.168.10.4"  # Уникальный IP
  }
}
resource "openstack_networking_port_v2" "WEB02_port2" {
  name           = "WEB02-port2"
  network_id     = openstack_networking_network_v2.management_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.management_subnet.id
    ip_address = "192.168.10.68"  # Измененный уникальный IP
  }
}


# Создание ВМ Cloud-DB01
resource "openstack_compute_instance_v2" "cloud_DB01" {
  name            = "Cloud-DB01"
  image_id        = var.image_id
  flavor_id       = var.flavor_id2  # Используем второй шаблон
  key_pair        = var.key_pair_name
  security_groups = ["default", "icmp"]

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    destination_type      = "volume"
    volume_size           = 10
    boot_index            = 0
    delete_on_termination = true
  }

  network {
    port = openstack_networking_port_v2.DB01_port.id
  }
network {
    port = openstack_networking_port_v2.DB01_port2.id
  }
}

resource "openstack_networking_port_v2" "DB01_port" {
  name           = "DB01-port"
  network_id     = openstack_networking_network_v2.internal_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.internal_subnet.id
    ip_address = "192.168.10.5"  # Уникальный IP
  }
}
resource "openstack_networking_port_v2" "DB01_port2" {
  name           = "DB01-port2"
  network_id     = openstack_networking_network_v2.management_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.management_subnet.id
    ip_address = "192.168.10.69"  # Измененный уникальный IP
  }
}

# Создание ВМ Cloud-DB02
resource "openstack_compute_instance_v2" "cloud_DB02" {
  name            = "Cloud-DB02"
  image_id        = var.image_id
  flavor_id       = var.flavor_id2  # Используем второй шаблон
  key_pair        = var.key_pair_name
  security_groups = ["default", "icmp"]

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    destination_type      = "volume"
    volume_size           = 10
    boot_index            = 0
    delete_on_termination = true
  }

  network {
    port = openstack_networking_port_v2.DB02_port.id
  }
network {
    port = openstack_networking_port_v2.DB02_port2.id
  }
}
resource "openstack_networking_port_v2" "DB02_port" {
  name           = "DB02-port"
  network_id     = openstack_networking_network_v2.internal_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.internal_subnet.id
    ip_address = "192.168.10.6"  # Уникальный IP
  }
}
resource "openstack_networking_port_v2" "DB02_port2" {
  name           = "DB02-port2"
  network_id     = openstack_networking_network_v2.management_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.management_subnet.id
    ip_address = "192.168.10.70"  # Измененный уникальный IP
  }
}
# Создание ВМ Cloud-DB02
resource "openstack_compute_instance_v2" "Cloud-MON" {
  name            = "Cloud-MON"
  image_id        = var.image_id
  flavor_id       = var.flavor_id2  # Используем второй шаблон
  key_pair        = var.key_pair_name
  security_groups = ["default", "icmp"]

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    destination_type      = "volume"
    volume_size           = 10
    boot_index            = 0
    delete_on_termination = true
  }

  network {
    port = openstack_networking_port_v2.MON_port.id
  }
network {
    port = openstack_networking_port_v2.MON_port2.id
  }
}
resource "openstack_networking_port_v2" "MON_port" {
  name           = "MON-port"
  network_id     = openstack_networking_network_v2.backup_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.backup_subnet.id
    ip_address = "192.168.10.129"  # Уникальный IP
  }
}
resource "openstack_networking_port_v2" "MON_port2" {
  name           = "MON-port2"
  network_id     = openstack_networking_network_v2.management_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.management_subnet.id
    ip_address = "192.168.10.71"  # Измененный уникальный IP
  }
}
# Создание ВМ Cloud-DB02
resource "openstack_compute_instance_v2" "Cloud-BACKUP01" {
  name            = "Cloud-BACKUP01"
  image_id        = var.image_id
  flavor_id       = var.flavor_id2  # Используем второй шаблон
  key_pair        = var.key_pair_name
  security_groups = ["default", "icmp"]

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    destination_type      = "volume"
    volume_size           = 10
    boot_index            = 0
    delete_on_termination = true
  }

  network {
    port = openstack_networking_port_v2.BACKUP01_port.id
  }
network {
    port = openstack_networking_port_v2.BACKUP01_port2.id
  }
}
resource "openstack_networking_port_v2" "BACKUP01_port" {
  name           = "BACKUP01-port"
  network_id     = openstack_networking_network_v2.backup_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.backup_subnet.id
    ip_address = "192.168.10.131"  # Уникальный IP
  }
}
resource "openstack_networking_port_v2" "BACKUP01_port2" {
  name           = "BACKUP01-port2"
  network_id     = openstack_networking_network_v2.management_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.management_subnet.id
    ip_address = "192.168.10.73"  # Измененный уникальный IP
  }
}
# Создание ВМ Cloud-DB02
resource "openstack_compute_instance_v2" "Cloud-BACKUP02" {
  name            = "Cloud-BACKUP02"
  image_id        = var.image_id
  flavor_id       = var.flavor_id2  # Используем второй шаблон
  key_pair        = var.key_pair_name
  security_groups = ["default", "icmp"]

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    destination_type      = "volume"
    volume_size           = 10
    boot_index            = 0
    delete_on_termination = true
  }

  network {
    port = openstack_networking_port_v2.BACKUP02_port.id
  }
network {
    port = openstack_networking_port_v2.BACKUP02_port2.id
  }
}
resource "openstack_networking_port_v2" "BACKUP02_port" {
  name           = "BACKUP02-port"
  network_id     = openstack_networking_network_v2.backup_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.backup_subnet.id
    ip_address = "192.168.10.132"  # Уникальный IP
  }
}
resource "openstack_networking_port_v2" "BACKUP02_port2" {
  name           = "BACKUP02-port2"
  network_id     = openstack_networking_network_v2.management_net.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.management_subnet.id
    ip_address = "192.168.10.74"  # Измененный уникальный IP
  }
}
# adm.tf
data "openstack_compute_instance_v2" "cloud_adm" {
id = "7a342e81-f13d-4426-8311-b8c7957819f3"
}
resource "openstack_networking_port_v2" "adm_mgmt_port" {
   name = "adm-mgmt-port"
   network_id = openstack_networking_network_v2.management_net.id
   admin_state_up = "true"
fixed_ip {
   subnet_id = openstack_networking_subnet_v2.management_subnet.id
   ip_address = "192.168.10.72"
}
}
resource "openstack_compute_interface_attach_v2" "adm_mgmt_attach" {
   instance_id = data.openstack_compute_instance_v2.cloud_adm.id # Используем полученный ID
   port_id = openstack_networking_port_v2.adm_mgmt_port.id
}
