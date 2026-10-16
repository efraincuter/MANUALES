### Historial en Ubuntu 24, aumentar tamaño
```
Editar el archivo bash bashrc

  desarrollo@srv-rep-sen:~$ vim  ~/.bashrc

Cambiar las siguientes vairables y adioncar la 3ra
  HISTSIZE=100000              -> Memoria durante la sesion
  HISTFILESIZE=200000          -> Comandos guardados en el ~/.bash_history
  HISTTIMEFORMAT="%F %T "

```
