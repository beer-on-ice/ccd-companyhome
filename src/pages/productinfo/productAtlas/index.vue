<template lang="pug">
  div#canvas(ref="canvas")
</template>

<script>
import init from './force'
export default {
  name: 'product-atlas',
  mounted () {
    this.getDataSource()
  },
  methods: {
    async getDataSource () {
      let param = {
        id: this.$route.query.id,
        nodeType: this.$route.query.nodeType
      }

      this.$api.productinfo
        .getD3DataSource(param)
        .then(res => {
          var node = []
          var relationships = []
          var nodes = []
          var relations = []

          var datas = JSON.parse(res).results[0].data

          for (var i = 0; i < datas.length; i++) {
            node.push(datas[i].graph.nodes)
            relationships.push(datas[i].graph.relationships)
          }
          for (var a = 0; a < node.length; a++) {
            // 节点
            for (var b = 0; b < node[a].length; b++) {
              nodes.push(node[a][b])
            }
          }
          for (let list in nodes) {
            // 修改nodes的结构
            for (let listSmall in nodes[list]) {
              if (listSmall === 'labels') {
                var ds = nodes[list][listSmall][0] // 取出labels数组的值

                delete nodes[list][listSmall]
                nodes[list][listSmall] = ds
              }

              if (listSmall === 'properties') {
                var name = nodes[list][listSmall].name
                var title = nodes[list][listSmall].full_name

                if (name) {
                  delete nodes[list][listSmall]
                  nodes[list][listSmall] = name
                } else if (title) {
                  delete nodes[list][listSmall]
                  nodes[list][listSmall] = title
                }
              }
            }
          }

          for (var t = 0; t < relationships.length; t++) {
            for (var y = 0; y < relationships[t].length; y++) {
              var flag = true

              for (var c = 0; c < relations.length; c++) {
                if (relationships[t][y].id === relations[c].id) {
                  flag = false
                }
              }
              if (flag) {
                relations.push(relationships[t][y])
              }
              if (relationships[t]) {
              }
            }
          }

          for (let listTwo in relations) {
            for (let lisTwoSamll in relations[listTwo]) {
              if (lisTwoSamll === '') {
                if (lisTwoSamll === 'type') {
                  console.log(relations[listTwo][lisTwoSamll])
                  if (relations[listTwo][lisTwoSamll] === '') {
                  }
                  //            if(relations[listTwo][lisTwoSamll]!=="{}"){
                  //            console.log(relations[listTwo][lisTwoSamll])
                  //            }
                  //             // for(proList in lisTwoSamll){
                  //             //     console.log(lisTwoSamll[proList])
                  //             // }
                }
              }
            }
          }

          return {
            nodes: nodes,
            relations: relations
          }
        })
        .then(resp => {
          init(this.$refs.canvas, resp)
        })
    }
  }
}
</script>

<style lang="scss">
.basic-layout-wrapper {
	height: 100%;
}
#canvas {
	height: 100%;
}

svg {
	background: rgb(245, 245, 245);
}
</style>
