# получение данных о существующих сетях
``` hcl
data "openstack_networking_network_v2" "имя сети" {
name = "имя сети"
}

# Создание сетей
resource "openstack_networking_network_v2" "название" {
name = "название сети"
admin_state_up = "true"
}

# Создание подсетей
resource "openstack_networking_subnet_v2" "название" {
name = "название подсети"
network_id = openstack_networking_network_v2.название_сети.id
cidr = "адресное пространство"
ip_version = ( 4 или 6)
gateway_ip = "адрес шлюза по умолчанию"
}

# Подключение подсетей к маршрутизатору
resource "openstack_networking_router_interface_v2" "название" {
router_id = openstack_networking_router_v2.название_маршрутизатора.id
subnet_id = openstack_networking_port_v2.название_подсети.id
}
