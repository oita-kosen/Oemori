<template>
  <canvas class="container"
          id="canvas"
          width="1024px" height="1024px"
          @mousemove="(e) => onMouseMove(e)">
  </canvas>
</template>

<script>
const update_dt_min = 10
export default {
  data() {
    // data関数: コンポーネントで使う変数の初期化など
    return {
      isStarted: false,
      lastDrawTime: 0,
      canvas: null,
      context: null
    }
  },
  mounted() {
    // mounted: ビューがマウントされたときに呼ばれるコールバック
    // 要するに Start() 関数

    // canvasのコンテキスト(描画の"ペン"のようなもの)を取得
    // ペンを動かす事で描画をしていく
    this.canvas = document.getElementById('canvas')
    this.context = canvas.getContext('2d')
  },
  methods: {
    onMouseMove(e) {
      // マウスが動いたときに呼ばれるコールバック
      // <canvas> タグにイベントに対するコールバックを設定している

      const rect = e.target.getBoundingClientRect()
      let mouseX = e.clientX - rect.left
      let mouseY = e.clientY - rect.top

      // 最後に描画してから一定時間立たないと更新しない
      let now = new Date().getTime() //Unixtime[ms]
      // console.log(now, this.lastDrawTime);
      if (now - this.lastDrawTime < update_dt_min) {
        return
      }
      this.lastDrawTime = now
      console.log("cursor:", mouseX, mouseY)

      if (this.isStarted) {
        this.context.strokeStyle = 'rgb(0,0,255)' //線の色は青
        this.context.lineWidth = 1;
        this.context.lineTo(mouseX, mouseY)
        this.context.stroke() // 現在のパスを輪郭表示
      } else {
        // まだ描画を開始していない場合
        this.context.beginPath() // パスを開始
        this.context.moveTo(mouseX, mouseY) // パスの開始座標を指定
        this.isStarted = true
      }
    } 
  }
}
</script>

<style lang="stylus" scoped>
.container {
}
</style>