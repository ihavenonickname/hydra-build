<script setup>
import { ref } from 'vue'
import HttpPostForm from '@/components/services/HttpPostForm.vue'

const service = ref('http-post-form')

const emit = defineEmits(['update'])
</script>

<template>
  <div class="block">
    <section class="field-group">
      <h2 class="title is-4 has-text-centered">Services options</h2>

      <div class="field is-horizontal">
        <div class="field-label is-normal">
          <label class="label">Service</label>
        </div>

        <div class="field-body">
          <div class="field">
            <div class="control">
              <div class="select">
                <select v-model="service">
                  <option>adam6500</option>
                  <option>afp</option>
                  <option>asterisk</option>
                  <option>cisco</option>
                  <option>cisco-enable</option>
                  <option>cvs</option>
                  <option>firebird</option>
                  <option>ftp</option>
                  <option>ftps</option>
                  <option>http-head</option>
                  <option>http-get</option>
                  <option>http-post</option>
                  <option>https-head</option>
                  <option>https-get</option>
                  <option>https-post</option>
                  <option>http-get-form</option>
                  <option>http-post-form</option>
                  <option>https-get-form</option>
                  <option>https-post-form</option>
                  <option>http-proxy</option>
                  <option>http-proxy-urlenum</option>
                  <option>icq</option>
                  <option>imap</option>
                  <option>imaps</option>
                  <option>irc</option>
                  <option>ldap2</option>
                  <option>ldap2s</option>
                  <option>ldap3</option>
                  <option>ldap3-crammd5</option>
                  <option>ldap3-digestmd5</option>
                  <option>ldap3s</option>
                  <option>ldap3-crammd5s</option>
                  <option>ldap3-digestmd5s</option>
                  <option>mssql</option>
                  <option>mysql(v4)</option>
                  <option>mysql5</option>
                  <option>ncp</option>
                  <option>nntp</option>
                  <option>oracle</option>
                  <option>oracle-listener</option>
                  <option>oracle-sid</option>
                  <option>pcanywhere</option>
                  <option>pcnfs</option>
                  <option>pop3</option>
                  <option>pop3s</option>
                  <option>postgres</option>
                  <option>rdp</option>
                  <option>radmin2</option>
                  <option>redis</option>
                  <option>rexec</option>
                  <option>rlogin</option>
                  <option>rpcap</option>
                  <option>rsh</option>
                  <option>rtsp</option>
                  <option>s7-300</option>
                  <option>sapr3</option>
                  <option>sip</option>
                  <option>smb</option>
                  <option>smtp</option>
                  <option>smtps</option>
                  <option>smtp-enum</option>
                  <option>snmp</option>
                  <option>socks5</option>
                  <option>ssh</option>
                  <option>sshkey</option>
                  <option>svn</option>
                  <option>teamspeak</option>
                  <option>telnet</option>
                  <option>telnets</option>
                  <option>vmauthd</option>
                  <option>vnc</option>
                  <option>xmpp</option>
                </select>
              </div>
              <p class="help">A service (also called protocol) to attack.</p>
            </div>
          </div>
        </div>
      </div>

      <template v-if="service === 'http-post-form'">
        <HttpPostForm @update="emit('update', $event)" />
      </template>
      <p v-else class="has-text-centered is-size-3">
        Service not implemented yet!
      </p>
    </section>
  </div>
</template>
