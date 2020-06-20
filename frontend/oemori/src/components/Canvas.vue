<template>
  <!-- VueのComponentファイルは，
    コンポーネントに必要なtemplate(html), style(css), script(js)が一つのファイルにまとめられている．
    - templateで 骨組み（大まかなページのレイアウト）
    - styleでフォントや位置・サイズ等の具体的な装飾を行う
    - scriptでアニメーション等の内部動作
    という役割分担がある．
    ここではざっくりいうとカーソルの位置に線を書いたりする処理のプログラムが大半を占めるので
    とりあえずプログラムのほとんどは scriptに書けば良い．
    （それ以外の見た目の調整はあとでいい）
  -->
  <canvas
    class="container"
    id="canvas"
    :width="canvasWidth"
    :height="canvasHeight"
    @mousedown="onMouseDown"
    @mousemove="onMouseMove"
    @mouseup="onMouseUp"
    @mouseleave="onMouseUp"
  ></canvas>
</template>

<script>
const update_dt_min = 10;
export default {
  data() {
    // data関数: コンポーネントで使う変数の初期化など
    return {
      isStarted: false,
      lastDrawTime: 0,
      canvas: null,
      context: null,
      isMousePress: false,
    };
  },
  props: {
    // コンポーネントが外部から受け取る引数の定義

    lineColor: {
      type: String,
      default: "rgb(0, 0, 255)",
    },
    lineWidth: {
      type: Number,
      default: 3,
    },
    mode: {
      type: String,
      default: "pen",
    },
    canvasWidth: {
      type: Number,
      default: 1024,
    },
    canvasHeight: {
      type: Number,
      default: 1024,
    },
  },
  mounted() {
    // mounted: ビューがマウントされたときに呼ばれるコールバック
    // 要するに Start() 関数

    // canvasのコンテキスト(描画の"ペン"のようなもの)を取得
    // ペンを動かす事で描画をしていく
    this.canvas = document.getElementById("canvas");
    this.context = canvas.getContext("2d");
  },
  methods: {
    // methods内に自作の関数を定義していく．
    onMouseDown(e) {
      //マウスが押されたときに呼ばれるコールバック
      this.isMousePress = true;
      console.log("mode:", this.mode);
    },
    onMouseMove(e) {
      // マウスが動いたときに呼ばれるコールバック
      // <canvas> タグにイベントに対するコールバックを設定している

      //マウスが押されてなければ何もしない
      if (this.isMousePress === false) return;

      const rect = e.target.getBoundingClientRect();
      let mouseX = e.clientX - rect.left;
      let mouseY = e.clientY - rect.top;

      // 最後に描画してから一定時間立たないと更新しない
      let now = new Date().getTime(); //Unixtime[ms]
      // console.log(now, this.lastDrawTime);
      if (now - this.lastDrawTime < update_dt_min) {
        return;
      }
      this.lastDrawTime = now;
      console.log("cursor:", mouseX, mouseY);

      if (this.isStarted) {
        //色を指定
        switch (this.mode) {
          case "pen":
            this.context.strokeStyle = this.lineColor;
            break;
          case "eraser":
            this.context.strokeStyle = "rgb(255, 255, 255)";
            break;
        }
        this.context.lineWidth = this.lineWidth;
        this.context.lineTo(mouseX, mouseY);
        this.context.stroke(); // 現在のパスを輪郭表示
      } else {
        // まだ描画を開始していない場合
        this.context.beginPath(); // パスを開始
        this.context.moveTo(mouseX, mouseY); // パスの開始座標を指定
        this.isStarted = true;
      }
    },
    onMouseUp(e) {
      this.isMousePress = false;
      this.isStarted = false;
    },
  },
};
</script>

<style lang="stylus" scoped>
.container {
}
</style>
