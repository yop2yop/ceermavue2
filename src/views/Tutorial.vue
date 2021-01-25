<template>
  <div class="tutorial" id="app">
    <h1>  Tutorial  </h1>
    <div class="text-center">
      <p> {{ $t('tutorial.firstparagraph') }}  </p> 
        
      <p> {{ $t('tutorial.secondparagraph') }}  </p> 

      <p> {{ $t('tutorial.thirdparagraph') }}  </p> 

      <p>
      <v-btn @click="createKernel"> {{ $t('tutorial.btn.connect') }} </v-btn>
      <v-btn @click="shutdownKernel"> {{ $t('tutorial.btn.shutdown') }} </v-btn>
      </p>

      <br />
      <textarea v-model="code"></textarea>
      <br />
      <v-btn @click="runCode">  {{ $t('tutorial.btn.run') }} </v-btn>
      <p>{{ streamResult }}</p>
      <p>{{ executeResult }}</p>
      
    </div>
  </div>
</template>

<script>
import { ServerConnection, Kernel } from '@jupyterlab/services'

export default {
  name: 'Tutorial',
  components: {
  },



  data() {
    return {
      jupyterKernel: null,
      code: null,
      streamResult: null,
      executeResult: null,
    }
  },



  methods: {
    createKernel() {
      let serverSettings = ServerConnection.makeSettings()
      serverSettings.baseUrl = 'http://localhost:8888'
      serverSettings.wsUrl = `ws://localhost:8888`
      serverSettings.pageUrl = 'http://localhost:8888'
      serverSettings.token = ''

      let options = {
        serverSettings: serverSettings,
        name: 'python',
        username: 'mohammad'
      }

      console.log('making a kernel...')
      Kernel.startNew(options).then(k => {
        console.log(k)
        this.jupyterKernel = k
      }).catch(err => {
        console.log(err)
      })
    },

    // -----------------------------------
    shutdownKernel() {
      console.log('start shutting down...')
      if (this.jupyterKernel) {
        this.jupyterKernel.shutdown()
        .then(() => {
          console.log('kernel shutdown...')
          console.log(this.jupyterKernel)
        })
        .catch(err => {
          console.log(err)
        })
        .then(() => {
          console.log('remove jupyter kernel')
          this.jupyterKernel = null
          console.log(this.jupyterKernel)
        })
      }
    },

    // -------------------------------------
    runCode() {
      console.log('start running code...')

      let future = this.jupyterKernel.requestExecute({ code: this.code })
      console.log(future)

      future.onIOPub = msg => {
        console.log(msg)

        if (msg.msg_type == 'stream') {
          this.streamResult = msg.content['text']
        }


        if(msg.msg_type == 'execute_result') {
          this.executeResult = msg.content.data['text/plain']
        }

        
      }
    }
  },
}
</script>
