============================
radsecproxy setup for eduroam
============================

------------
Introduction
------------

*radsecproxy is a generic RADIUS proxy that in addition to usual RADIUS UDP transport, also supports TLS (RadSec), as well as RADIUS over TCP and DTLS. The aim is for the proxy to have sufficient features to be flexible, while at the same time to be small, efficient and easy to configure.*

*The proxy was initially made to be able to deploy RadSec (RADIUS over TLS) so that all RADIUS communication across network links could be done using TLS, without modifying existing RADIUS software. This can be done by running this proxy on the same host as an existing RADIUS server or client, and configure the existing client/server to talk to localhost (the proxy) rather than other clients and servers directly.*

*There are however other situations where a RADIUS proxy might be useful. Some people deploy RADIUS topologies where they want to route RADIUS messages to the right server. The nodes that do purely routing could be using a proxy. Some people may also wish to deploy a proxy on a site boundary. Since the proxy supports both IPv4 and IPv6, it could also be used to allow communication in cases where some RADIUS nodes use only IPv4 and some only IPv6.*

-----------------
About this how-to
-----------------

This example should provide a general overview of the radsecproxy-plugin for OPNsense, but also explain how to use radsecproxy in conjunction with `eduroam <https://eduroam.org/>`_, an international roaming service for users in research and education. The eduroam-manual this how-to is based on can be found `here <https://www.dfn.de/fileadmin/1Dienstleistungen/Roaming/Einrichtung_von_radsecproxy.pdf>`_.

----------------------------
About the radsecproxy-plugin
----------------------------

Radsecproxy is a gui-less daemon which loads its configuration from the ``radsecproxy.conf`` file. To make it as easy as possible to transfer configuration-examples to this plugin, the plugins is orientated on the configfile's structure. Detailed information about the possible settings are provided in the official `manpage <https://radsecproxy.github.io/radsecproxy.conf.html>`_.

Install the plugin via :menuselection:`System --> Firmware --> Plugins`, selecting **os-radsecproxy**. Once the plugin is installed, refresh the browser page and you will find the RadSecProxy configuration menu via :menuselection:`Services --> RadSecProxy`.

#################
Section *General*
#################

Global settings like loglevels, privacy-options, listening- and source-addresses are configured in this section. 

$$$$$$$$$$$$$$$$$$
Example p. 14 - 15
$$$$$$$$$$$$$$$$$$

- Go to :menuselection:`Services --> RadSecProxy --> General`
- Configure the general settings as follows (if an option is not mentioned below, leave it as the default):

    ===================== ===============================================================================================
     **Enabled**           *Checked*
     **Loglevel**          *1*
     **Listen UDP**        *127.0.0.1:2084  or another socket to listen for unencrypted (plain radius) requests*
     **Listen TLS**        *192.168.1.1:2083 or another socket to listen for encrypted (radsec) requests*
    ===================== ===============================================================================================

#################
Section *TLS*
#################

All the other section are the so called *blocks*. Each block can contain one or more settings, whereas at least the blocks *Clients* and *Realms* must be provided. The TLS-block provides the configuration for the encrypted communication.

$$$$$$$$$$$$$$$$$$
Example p. 15
$$$$$$$$$$$$$$$$$$

- Go to :menuselection:`Services --> RadSecProxy --> TLS`
- Click **+** to add a new TLS-configuration
- Configure the settings as follows (if an option is not mentioned below, leave it as the default):

    =============================== ====================================================================================================
     **Unique name**                 *Provide a unique name to identify this configuration. If it is named 'default', it will be used if other blocks don't specify an explicit TLS-config*
     **Description**                 *Describe your config-block like 'My Company's TLS-settings'*
     **CA-certificate**              *Select a previously imported CA-certificate, which radsecproxy uses to verify the peers certificate*
     **This server's certificate**   *Select a previously imported certificate, which this proxy will use. The file may also contain a certificate chain.*
    =============================== ====================================================================================================

.. Note::
    The proxy-certificates and CA-certificates must be imported before configuring this TLS-block via :menuselection:`System --> Trust --> Certificates` and :menuselection:`System --> Trust --> Authorities`.
