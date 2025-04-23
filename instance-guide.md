# Создание виртуальных машин
подробное описание порцесса создания Виртуальных машин
```hcl
resource "openstack_compute_instance_v2" "Cloud_HA01" {
  name            = "имя_машины"
  image_id        = "id или название образа" 
  flavor_id       = "название или id шаблона"
  key_pair        = "название или id ключевой пары для доступа к ВМ"
  security_groups = ["название группы безопасности"]
```
# создание сетевых интерфейсов и подключение их к ВМ
```
resource "openstack_networking_port_v2" "название" {
  name           = "название_порта"
  network_id     = openstack_networking_network_v2.название_сети.id
  admin_state_up = "true"

  fixed_ip {
    subnet_id  = openstack_networking_subnet_v2.название_подсети.id
    ip_address = "ip адрес ВМ"
  }
}
  network {
    port = openstack_networking_port_v2.название_порта.id
  }
  ```
# Добавление информации о существующих ВМ
```
data "openstack_compute_instance_v2" "название_ВМ" {
id = "ID_виртуальной_машины"
}
```
