<template>
  <div class="window" v-on:click="closeCart()">
    <div class="header">
      <div class="header-left-container">
        <img src="@/assets/icon-menu.svg" alt="menu mobile" class="icone-menu-mobile" v-on:click="showMenu()">
        <div :style="menuStyle()">
          <span class="material-icons close" v-on:click="closeMenu()">
            close
          </span>
          <div class="nav-mobile">Collections</div>
          <div class="nav-mobile">Men</div>
          <div class="nav-mobile">Women</div>
          <div class="nav-mobile">About</div>
          <div class="nav-mobile">Contact</div>
        </div>
        <img src="@/assets/logo.svg" alt="Logo Sneakers" class="logo">
        <div class="nav-container">
          <div class="nav-link">Collections</div>
          <div class="nav-link">Men</div>
          <div class="nav-link">Women</div>
          <div class="nav-link">About</div>
          <div class="nav-link">Contact</div>
        </div>
      </div>
      <div class="user">
        <div class="container-dropdown">
          <div class="cart-counter" v-if="cartUnit != 0">
            {{ cartUnit }}
          </div>
          <span class="material-icons car" v-on:click.stop="showCart()">
            shopping_cart
          </span>
          <div :class="cartStyle()">
            <div class="cart-title">
              Cart
            </div>
            <div class="cart-content">
              <div v-if="cartUnit === 0">
                Your cart is empty
              </div>
              <div class="product-container" v-else>
                <div class="product-in-cart">
                  <img src="@/assets/image-product-1-thumbnail.jpg" class="product-thumb">
                  <div>
                    Fall Limited Edition Sneakers
                    <div class="total-sum">
                      $125.00 x {{ cartUnit }} =
                      <div class="total-price">
                        ${{ total }}.00
                      </div>
                    </div>
                  </div>
                  <span class="material-icons delete" v-on:click.stop="deleteCart()">
                    delete
                  </span>
                </div>
                <button class="cart-btn">
                  Checkout
                </button>
              </div>
            </div>
          </div>
        </div>
        <img src="@/assets/image-avatar.png" alt="icone avatar" class="avatar">
      </div>
    </div>
    <div class="body">
      <div class="product-view">
        <div class="big-card" v-on:click.stop="openFullScreen()">
          <img src="@/assets/icon-previous.svg" class="previous" v-on:click.stop="previousProductMobile()">
          <img src="@/assets/icon-next.svg" class="next" v-on:click.stop="nextProductMobile()">
          <img src="@/assets/image-product-1.jpg" class="thumb-img" v-if="index === 0">
          <img src="@/assets/image-product-2.jpg" class="thumb-img" v-if="index === 1">
          <img src="@/assets/image-product-3.jpg" class="thumb-img" v-if="index === 2">
          <img src="@/assets/image-product-4.jpg" class="thumb-img" v-if="index === 3">
        </div>
        <div class="card-preview">
          <img src="@/assets/image-product-1-thumbnail.jpg" class="preview" v-on:click="changeProduct1()"
            :style="imgSelected1()">
          <img src="@/assets/image-product-2-thumbnail.jpg" class="preview" v-on:click="changeProduct2()"
            :style="imgSelected2()">
          <img src="@/assets/image-product-3-thumbnail.jpg" class="preview" v-on:click="changeProduct3()"
            :style="imgSelected3()">
          <img src="@/assets/image-product-4-thumbnail.jpg" class="preview" v-on:click="changeProduct4()"
            :style="imgSelected4()">
        </div>
      </div>
      <div class="product-info">
        <div class="company">
          SNEAKER COMPANY
        </div>
        <div class="title">
          Fall Limited Edition Sneakers
        </div>
        <div class="content">
          These low-profile sneakers are your perfect casual wear companion. Featuring a durable rubber outer sole,
          they'll witstand everything the weather can offer.
        </div>
        <div class="price">
          $125.00
          <div class="discount">
            50%
          </div>
        </div>
        <div class="old-price">
          $250.00
        </div>
        <div class="amount">
          <div class="counter">
            <img src="@/assets/icon-minus.svg" class="minus" v-on:click="btnMinus()">
            {{ unitCounter }}
            <img src="@/assets/icon-plus.svg" class="plus" v-on:click="btnPlus()">
          </div>
          <button v-on:click="addToCart()">
            <span class="material-icons cart-button">
              add_shopping_cart
            </span>
            Add to cart
          </button>
        </div>
      </div>
    </div>
    <div class="container-full-screen" v-if="fullScreen === true">
      <span class="material-icons close-full-screen" v-on:click="closeFullScreen()">
        close
      </span>
      <div class="view">
        <span class="material-icons next-full-screen" v-on:click="nextProduct()">
          navigate_next
        </span>
        <span class="material-icons previous-full-screen" v-on:click="previousProduct()">
          navigate_before
        </span>
        <img src="@/assets/image-product-1.jpg" class="thumb-img" v-if="indexFullScreen === 0">
        <img src="@/assets/image-product-2.jpg" class="thumb-img" v-if="indexFullScreen === 1">
        <img src="@/assets/image-product-3.jpg" class="thumb-img" v-if="indexFullScreen === 2">
        <img src="@/assets/image-product-4.jpg" class="thumb-img" v-if="indexFullScreen === 3">
        <div class="mini-view">
          <img src="@/assets/image-product-1-thumbnail.jpg" class="preview" v-on:click="changeProduct1()"
            :style="imgSelectedFullScreen1()">
          <img src="@/assets/image-product-2-thumbnail.jpg" class="preview" v-on:click="changeProduct2()"
            :style="imgSelectedFullScreen2()">
          <img src="@/assets/image-product-3-thumbnail.jpg" class="preview" v-on:click="changeProduct3()"
            :style="imgSelectedFullScreen3()">
          <img src="@/assets/image-product-4-thumbnail.jpg" class="preview" v-on:click="changeProduct4()"
            :style="imgSelectedFullScreen4()">
        </div>
      </div>
    </div>
    <div class="darken" v-if="menu === true">
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      visibleCart: false,
      menu: false,
      unitCounter: 0,
      index: 0,
      cartUnit: 0,
      total: 0,
      fullScreen: false,
      indexFullScreen: 0
    }
  },
  methods: {
    openFullScreen() {
      this.fullScreen = true
    },
    closeFullScreen() {
      this.fullScreen = false
    },
    addToCart() {
      this.cartUnit = this.unitCounter + this.cartUnit
      this.total = this.cartUnit * 125.00
      this.unitCounter = 0
    },
    deleteCart() {
      this.cartUnit = 0
    },
    closeCart() {
      this.visibleCart = false
    },
    showCart() {
      if (this.visibleCart === false) {
        this.visibleCart = true
      } else {
        this.visibleCart = false
      }
    },
    cartStyle() {
      if (this.visibleCart === true) {
        return "cart-style"
      } else {
        return "no-style"
      }
    },
    showMenu() {
      if (this.menu === false) {
        this.menu = true
      }
    },
    closeMenu() {
      this.menu = false
    },
    menuStyle() {
      if (this.menu === true) {
        return "background-color: white;width: 50%;height: 100%;position:absolute;top: 0;left: 0; padding: 8vh 5vw; z-index:2;box-sizing:border-box"
      } else {
        return "display:none;"
      }
    },
    btnMinus() {
      this.unitCounter--
    },
    btnPlus() {
      this.unitCounter++
    },
    changeProduct1() {
      if (this.fullScreen === false) {
        this.index = 0
      } else {
        this.indexFullScreen = 0
      }
    },
    changeProduct2() {
      if (this.fullScreen === false) {
        this.index = 1
      } else {
        this.indexFullScreen = 1
      }
    },
    changeProduct3() {
      if (this.fullScreen === false) {
        this.index = 2
      } else {
        this.indexFullScreen = 2
      }
    },
    changeProduct4() {
      if (this.fullScreen === false) {
        this.index = 3
      } else {
        this.indexFullScreen = 3
      }
    },
    nextProduct() {
      if (this.indexFullScreen <= 2) {
        this.indexFullScreen++
      } else {
        this.indexFullScreen = 0
      }
    },
    previousProduct() {
      if (this.indexFullScreen >= 1) {
        this.indexFullScreen--
      } else {
        this.indexFullScreen = 3
      }
    },
    nextProductMobile() {
      if (this.index <= 2) {
        this.index++
      } else {
        this.index = 0
      }
    },
    previousProductMobile() {
      if (this.index >= 1) {
        this.index--
      } else {
        this.index = 3
      }
    },
    imgSelected1() {
      if (this.index == 0) {
        return "opacity: .5; outline: 3px solid hsl(26, 100%, 55%);"
      }
    },
    imgSelected2() {
      if (this.index === 1) {
        return "opacity: .5; outline: 3px solid hsl(26, 100%, 55%);"
      }
    },
    imgSelected3() {
      if (this.index === 2) {
        return "opacity: .5; outline: 3px solid hsl(26, 100%, 55%);"
      }
    },
    imgSelected4() {
      if (this.index === 3) {
        return "opacity: .5; outline: 3px solid hsl(26, 100%, 55%);"
      }
    },
    imgSelectedFullScreen1() {
      if (this.indexFullScreen == 0) {
        return "opacity: .5; outline: 3px solid hsl(26, 100%, 55%);"
      }
    },
    imgSelectedFullScreen2() {
      if (this.indexFullScreen == 1) {
        return "opacity: .5; outline: 3px solid hsl(26, 100%, 55%);"
      }
    },
    imgSelectedFullScreen3() {
      if (this.indexFullScreen == 2) {
        return "opacity: .5; outline: 3px solid hsl(26, 100%, 55%);"
      }
    },
    imgSelectedFullScreen4() {
      if (this.indexFullScreen == 3) {
        return "opacity: .5; outline: 3px solid hsl(26, 100%, 55%);"
      }
    },
  }
}
</script>
<style>
body {
  margin: 0;
  font-family: 'Kumbh Sans', sans-serif;
}

.window {
  padding: 3vw 7vw;
  position: relative;
}

.darken {
  position: absolute;
  background-color: rgba(0,0,0, 0.7);
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;

}

.header {
  display: flex;
  border-bottom: 1px solid hsl(220, 14%, 75%);
  align-items: center;
  justify-content: space-between;
  height: 20vh;
  margin-bottom: 10vh;
}

.header-left-container {
  display: flex;
  align-items: center;
  gap: 5vw;
  height: 100%;
}

.nav-container {
  display: flex;
  align-items: center;
  gap: 2vw;
  color: hsl(220, 14%, 75%);
  cursor: pointer;
  height: 100%;
}

.nav-link {
  display: flex;
  align-items: center;
  height: 100%;
  font-weight: 700;
}

.nav-link:hover {
  color: hsl(220, 13%, 13%);
  border-bottom: 2px solid hsl(26, 100%, 55%);
}

.logo {
  height: 5vh;
}

.icone-menu-mobile {
  display: none;
}

.user {
  display: flex;
  align-items: center;
  gap: 2vw;
}

.cart-style {
  background-color: white;
  width: 27vw;
  height: 35vh;
  box-shadow: 5px 5px 10px #c1c1c1, -5px 5px 10px #c1c1c1, -5px 5px 10px #ffffff;
  position: absolute;
  top: 7vh;
  right: -8vw;
  z-index: 2;
}

.no-style {
  display: none;
}

.car {
  height: 4vh;
  padding: 0.5vh;
  cursor: pointer;
  border-radius: 50%;
  color: hsl(220, 14%, 75%);
  font-size: 2em;
  user-select: none;
}

.car:hover {
  color: hsl(26, 100%, 55%);
}

.container-dropdown {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
}

.cart-title {
  padding: 2vh 2vw;
  font-weight: 700;
  border-bottom: 1px solid hsl(220, 14%, 75%);
}

.cart-content {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 75%;
  font-weight: 700;
  color: hsl(220, 14%, 75%);
}

.cart-counter {
  padding: 0.1vh 0.8vh;
  border-radius: 50%;
  background-color: hsl(26, 100%, 55%);
  color: white;
  position: absolute;
  right: 0;
  top: -1vh;
}

.product-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.product-in-cart {
  display: flex;
  align-items: center;
  gap: 1vw;
  padding: 1vw;
}

.product-thumb {
  height: 10vh;
  border-radius: 10px;
}

.total-sum {
  display: flex;
  align-items: center;
  gap: 1vw;
}

.total-price {
  color: hsl(220, 13%, 13%);
  font-weight: 700;
}

.delete {
  font-size: 1.5em;
  cursor: pointer;
  height: fit-content;
}

.delete:hover {
  color: hsl(26, 100%, 55%);
  transition: ease-in-out;
}

.cart-btn {
  width: 90%;
  padding: 2vh 5vw;
  height: 8vh;
}

.avatar {
  height: 8vh;
  cursor: pointer;
}

.avatar:hover {
  outline: 1px solid red;
  border-radius: 50%;
}

.body {
  display: flex;
  gap: 5vw;
}

.product-view {
  width: 40vw;
}

.previous,
.next {
  display: none;
}

.big-card {
  width: 100%;
  border-radius: 10px;
  display: flex;
  overflow: hidden;
  cursor: pointer;
}

.thumb-img {
  width: 100%;
  height: auto;
  border-radius: 10px;
}

.card-preview {
  width: 100%;
  display: flex;
  gap: 2vw;
  margin-top: 5vh;
}

.preview {
  width: 8.5vw;
  border-radius: 10px;
  cursor: pointer;
  background-color: black;
}

.product-info {
  width: 40vw;
  display: flex;
  flex-direction: column;
  padding: 10vh 3vw;
  gap: 3vh;
}

.company {
  color: hsl(26, 100%, 55%);
  font-weight: 700;
}

.title {
  font-size: 3em;
  color: hsl(220, 13%, 13%);
  font-weight: 700;
  margin-bottom: 3vh;
}

.content {
  color: hsl(219, 9%, 45%);
}

.price {
  font-size: 2em;
  color: hsl(220, 13%, 13%);
  font-weight: 700;
  display: flex;
  align-items: center;
  gap: 2vw;
}

.discount {
  color: hsl(26, 100%, 55%);
  background-color: hsl(25, 75%, 87%);
  padding: 1vh 1vw;
  border-radius: 10px;
  font-size: 0.5em;
}

.old-price {
  font-size: 0.8em;
  color: hsl(219, 9%, 45%);
  margin-top: -3vh;
}

.amount {
  display: flex;
  align-items: center;
  gap: 1vw;
}

.counter {
  display: flex;
  align-items: center;
  width: 7vw;
  justify-content: space-between;
  background-color: hsl(232, 69%, 94%);
  border-radius: 10px;
  padding: 1vh 1vw;
}

.minus,
.plus {
  cursor: pointer;
  user-select: none;
  font-size: 2em;
}

button {
  background-color: hsl(26, 95%, 59%);
  border: none;
  padding: 1.5vh 4vw;
  border-radius: 10px;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1vw;
  font-weight: 400;
  font-size: 1em;
  cursor: pointer;
}

button:hover {
  background-color: hsl(26, 100%, 50%);

}

.container-full-screen {
  position: absolute;
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  top: 0;
  left: 0;
}

.view {
  width: 35vw;
  padding-top: 10vh;
  height: auto;
  position: relative;
}

.next-full-screen,
.previous-full-screen {
  position: absolute;
  top: 45vh;
  background-color: white;
  padding: 1vh;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.5em;
  user-select: none;
}

.next-full-screen {
  right: -1.4vw;
}

.previous-full-screen {
  left: -1.4vw;
}

.next-full-screen:hover,
.previous-full-screen:hover {
  color: hsl(26, 100%, 55%);
  transition: ease-in-out;
}

.mini-view {
  margin-top: 3vh;
  display: flex;
  height: 15vh;
  gap: 1.9vw;
}

.mini-view img {
  width: auto;
}

.close-full-screen {
  position: absolute;
  color: white;
  right: 30vw;
  top: 29vh;
  cursor: pointer;
  font-size: 2em;
}

.close-full-screen:hover {
  color: hsl(26, 100%, 55%);
  transition: ease-in-out;
}

img {
  user-select: none;
}


@media screen and (max-width:1000px) {
  .window {
    padding: 2vh 0;
  }

  .header {
    border: none;
    height: 5vh;
    justify-content: space-between;
    padding: 0 5vw;
    margin-bottom: 2vh;
  }

  .user {
    height: 100%;
  }

  .icone-menu-mobile {
    display: block;
    height: 1.5em;
  }

  .close {
    position: absolute;
    top: 3vh;
    left: 5vw;
  }

  .nav-mobile {
    font-weight: 700;
    margin-top: 2vh;
  }

  .nav-container {
    display: none;
  }

  .logo {
    height: 1.9em;
  }

  .avatar {
    height: 2.5em;
  }

  .car {
    font-size: 1.8em;
    margin-top: 1vh;
  }

  .cart-style {
    width: 95vw;
    height: 38vh;
    right: -14vw;
    top: 10vh;
    box-shadow: none;
    border-radius: 10px;
  }

  .cart-title {
    padding: 3.5vh;
  }

  .cart-counter {
    padding: 0 0.5vh;
    top: 0.5vh;
  }

  .product-container {
    width: 100%;
    gap: 3vw;
  }

  .product-in-cart {
    gap: 3vw;
  }

  .body {
    flex-direction: column;
  }

  .product-view {
    width: 100%;
  }

  .big-card {
    border-radius: none;
    position: relative;
    display: flex;
    align-items: center;
  }

  .thumb-img{
    border-radius: none;
  }

  .previous,
  .next {
    display: block;
    position: absolute;
    background-color: white;
    padding: 2vh;
    border-radius: 50%;
  }

  .next {
    right: 4vw;
  }

  .previous {
    left: 4vw;
  }

  .card-preview {
    display: none;
  }

  .product-info {
    width: 90%;
    padding: 0 4.9vw;
    gap: 1vh;
  }

  .title {
    font-size: 2.6em;
  }

  .content {
    width: 100%;
    line-height: 1.5em;
    font-size: 1.2em;
  }

  .old-price {
    margin-top: -1vh;
  }

  .amount {
    flex-direction: column;
  }

  .counter {
    width: 90%;
    padding: 2vh 4vw;
  }

  button {
    width: 100%;
    padding: 2vh 5vw;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 3vw;
  }
  .container-full-screen{
    display: none;
  }
}</style>