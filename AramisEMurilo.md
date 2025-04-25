
# Documentação - Configuração de Servidor de Log com Rsyslog

## Situação-Problema

A empresa fictícia **"RedesBR"** deseja centralizar os arquivos de log de **3 computadores da rede** para facilitar o monitoramento e auditoria. Para isso, será utilizado o serviço `rsyslog`.

---

## 1. Designação do Servidor de Log

- Servidor de Log: IP `192.168.1.10`

---

## 2. Configuração do Servidor (192.168.1.10)

### Etapas:

1. Editar o arquivo `/etc/rsyslog.conf` e descomentar ou adicionar as linhas:
    ```
    module(load="imudp")
    input(type="imudp" port="514")

    module(load="imtcp")
    input(type="imtcp" port="514")
    ```

2. Reiniciar o serviço:
    ```
    sudo systemctl restart rsyslog
    ```

3. Verificar se está escutando na porta 514:
    ```
    sudo netstat -tunlp | grep 514
    ```

---

## 3. Configuração dos Clientes (192.168.1.11, 192.168.1.12, 192.168.1.13)

### Etapas:

1. Criar o arquivo de configuração `/etc/rsyslog.d/remote.conf` com o conteúdo:
    ```
    *.* @@192.168.1.10:514
    ```

2. Reiniciar o serviço rsyslog:
    ```
    sudo systemctl restart rsyslog
    ```

---

## 4. Teste e Verificação

### Enviar mensagem de teste a partir do cliente:
```
logger "Teste de log remoto"
```

### Verificar no servidor:
```
sudo tail -f /var/log/syslog
```
ou
```
journalctl -f
```

---

## 5. Avaliação

| Critério                                 | Status       |
|------------------------------------------|--------------|
| Configuração correta do servidor         | ✅ Realizado |
| Configuração correta dos clientes        | ✅ Realizado |
| Funcionamento verificado                 | ✅ Log enviado e recebido com sucesso |
| Leitura de logs demonstrada              | ✅ Utilização de `journalctl` e `tail` |
| Documentação do processo                 | ✅ Completa |

---

**Atividade concluída com sucesso.**
