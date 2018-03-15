### **Scripting para Mikrotik** 

## Como eliminar una lista de direcciones (fw/address-list) en un solo comando
[Foro MKTK con la respuesta original](https://forum.mikrotik.com/viewtopic.php?t=93848)
```python
/ip firewall address-list remove [/ip firewall address-list find list="list-name"]
```

## Ver el contenido de un archivo en ROM
```python
/file print detail where name="file.ext"
```
[Pagina con buenos ejemplos de trabajo con files](https://www.mikrotik-routeros.com/2016/06/the-basics-of-reading-and-writing-files-in-routeros/)

## Firewall: estrategias contra ataques de fuerza bruta (fail2ban)
La idea es aprovechar una feature del FW de MKTK que permite ante un match agregar una ip
a una lista determinada.
En este caso vamos a ver si hay matches en un determinado tiempo mandamos la ip a una lista
y de esa a otra y al final la blacklisteamos.
 - [Idea original](https://wiki.mikrotik.com/wiki/Bruteforce_login_prevention)

```python
    /ip firewall filter
        add chain=input protocol=tcp dst-port=22 src-address-list=ssh_blacklist action=drop \
            comment="drop ssh brute forcers" disabled=no
        
        add chain=input protocol=tcp dst-port=22 connection-state=new \
            src-address-list=ssh_stage3 action=add-src-to-address-list address-list=ssh_blacklist \
            address-list-timeout=10d comment="" disabled=no
        
        add chain=input protocol=tcp dst-port=22 connection-state=new \
            src-address-list=ssh_stage2 action=add-src-to-address-list address-list=ssh_stage3 \
            address-list-timeout=1m comment="" disabled=no
        
        add chain=input protocol=tcp dst-port=22 connection-state=new src-address-list=ssh_stage1 \
            action=add-src-to-address-list address-list=ssh_stage2 address-list-timeout=1m comment="" disabled=no
        
        add chain=input protocol=tcp dst-port=22 connection-state=new action=add-src-to-address-list \
            address-list=ssh_stage1 address-list-timeout=1m comment="" disabled=no
```


## Como logearse a la CLI por SSH con la key
Tenemos que copiar nuestra key publica al router y ejecutar un comando para que la sume a la 
keychain.
[Ver en el link a la documentacion oficial](https://wiki.mikrotik.com/wiki/Use_SSH_to_execute_commands_(DSA_key_login))

 1- Copiamos la key con ftp

> ftp routermktk
    USER admin
    PASS *****
  > put id_rsa.pub

 2- Ahora en el router levantamos la key a la keychain
    ```python
    [admin@mikrotik]> user ssh-keys import file=id_dsa.pub 
    user: admin-ssh
    ```

# 
