Dell iDRAC TRAPs Zabbix Template.
============

1. O que é
---
Esse é um Template do Zabbix que trata TRAPs enviadas de placas iDRAC da Dell.

Esse Template foi testado em iDRACs de servidores Dell Power Edge R410, R420 e R430.

2. Premissas
---
* Snmptt

Assumo que você já tenha configurado seu servidor para manipular TRAPs através do software SNMPTT (http://http://snmptt.sourceforge.net/), caso ainda não tenha, utilize esse tutorial como base: (https://www.zabbix.com/documentation/2.4/manual/config/items/itemtypes/snmptrap)

* iDRAC

Assumo que você já tenha habilitado a geração de TRAPs em sua iDRAC.

**Atenção:** Esse template **NÃO** gera alertas para TRAPs com nível INFORMATIONAL, logo, cabe a você configurar sua iDRAC para gerar TRAPs apenas nos níveis WARNING e CRITICAL, caso contrário, você receberá muitos alertas "Unmatched TRAPs".

* Zabbix

Assumo que o *host* no Zabbix que representa sua iDRAC tem o endereço IP como identificação principal, isso é necessário pois, por algum motivo, o Zabbix não consegue identificar o HOST que gera a TRAP através do hostname.

3. Como usar
---
Clone o repositório do template, e copie o arquivo *iDRAC-430.conf* para, por exemplo, /etc/snmp

     git clone https://github.com/drequena/zabbix-iDracDellTraps

 Configure o snmptt para utilizar o arquivo que trata e faz logs das TRAPs da iDRAC

    vim /etc/snmp/snmptt.ini
    ...
    snmptt_conf_files = <<END
    /etc/snmp/iDRAC-430.conf
    /etc/snmp/snmptt.conf
    END

 Reinicie o serviço:

     systemctl restart snmptt

 Importe o XML do template dentro do Zabbix e associe ao host que representa a iDRAC do seu servidor.

4. Configuração de alertas
---

 É possível configurar o tempo o qual os alertas permanecerão ativos para TRAPs WARNING, CRITICAL e UNMATCHED pelas MACROS:
* {$CRITICAL_TIMEOUT} (default: 3600 segundos)
* {$WARNING_TIMEOUT} (default: 1800 segundos)
* {$UNMATCHED_TIMEOUT} (default: 600 segundos)
