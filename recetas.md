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
