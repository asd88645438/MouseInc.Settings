<template>
  <div>
    <p style="padding-bottom: 10px;">{{$t('list_tips1')}} <router-link to="settings">{{$t('list_tips2')}}</router-link> {{$t('list_tips3')}}</p>
    <Row :gutter="10">
      <Col :sm="12" :md="8" :lg="6" :xl="4" :xxl="3" v-for="(info, i) in cfg.Gestures" :key="`info-${i}`" style="padding-bottom: 10px;">
        <Card shadow>
          <Poptip
            confirm
            :title="$t('list_warning')"
            :transfer="true"
            style="position: absolute;right:13px;top:13px; cursor: pointer"
            @on-ok="remove(info)">
            <Icon type="md-close-circle" color="#d5d8de" size="18"/>
          </Poptip>
          <div style="text-align:center" class="gestures">
            <img :src="imgsrc(info)" style="width: 48px;height: 48px;">
            <Input :value="info.Sign" @on-change="change(info, $event)" />
          </div>
        </Card>
      </Col>
    </Row>
    <!-- {{gestures}} -->
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'list',
  methods: {
    imgsrc (info) {
      return this.gestures[info.Sign] ? this.gestures[info.Sign] : this.gestures['placeholder']
    },
    change (info, event) {
      var value = event.target.value
      this.gestures[value] = this.gestures[info.Sign]
      delete this.gestures[info.Sign]
      info.Sign = value
    },
    remove (info) {
      delete this.gestures[info.Sign]
      this.cfg.Gestures.splice(this.cfg.Gestures.findIndex(item => item.Sign === info.Sign), 1)
    }
  },
  computed: {
    ...mapGetters([
      'cfg',
      'gestures'
    ])
  }
}
</script>

<style>
.gestures input { text-align:center }
</style>
