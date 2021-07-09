<template>
  <div class="container">
    <div class="row">
      <Wrapper :rows="rows" class="wrapper" />
      <transition tag="div" mode="out-in" name="fade">
        <div class="start-section" v-if="!gameIsRunning">
          <button @click="startGame" class="btn btn-outline-primary">Start</button>
        </div>
        <div class="restart-section" v-else>
          <button @click="restartGame" class="btn btn-outline-primary">ReStart</button>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import Wrapper from './components/Wrapper.vue'

export default {
  name: 'App',
  data() {
    return {
      rows: [
        0, 0, 0, 0, 
        0, 0, 0, 0, 
        0, 0, 0, 0, 
        0, 0, 0, 0
      ],
      min: 0,
      max: 15,
      gameIsRunning: false,
      canGoNext: true,
    }
  },
  components: {
    Wrapper
  },
  methods: {
    randomIntFromRange(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    },
    nextNum() {
      let num = null;
      let flag = this.randomIntFromRange(1, 10);
      if(flag <= 6) {
        num = 2;
      } else {
        num = 4;
      }
      return num;
    },
    setValue(index, value) {
      this.rows[index] = value;
    },
    restartGame() {
      this.rows = [
        0, 0, 0, 0, 
        0, 0, 0, 0, 
        0, 0, 0, 0, 
        0, 0, 0, 0
      ],
      this.gameIsRunning = false;
      this.canGoNext = true;
    },
    startGame() {
      let firstIndex = this.randomIntFromRange(this.min, this.max);
      let secondIndex = this.randomIntFromRange(this.min, this.max);
      while( firstIndex === secondIndex ) {
        secondIndex = this.randomIntFromRange(this.min, this.max);
      }
      let firstNum = this.nextNum();
      let secondNum = this.nextNum();
      this.setValue(firstIndex, firstNum);
      this.setValue(secondIndex, secondNum);
      
      this.gameIsRunning = true;
      this.nextMove();
    },
    nextMove() {
      if(!this.gameIsRunning) {
        return;
      }
      this.canGoNext = true;
      if(this.canGoNext) {
        window.addEventListener('keydown', event => {
          let num = this.nextNum();
          let index = this.randomIntFromRange(this.min, this.max);
          while(this.rows[index] !== 0) {
            index = this.randomIntFromRange(this.min, this.max)
          }
          this.canGoNext = false;
          let first, last, distance, end, payload = null;
          switch (event.key) {
            case "ArrowDown":
              first = 0;
              last = 12;
              distance = 4;
              end = 4;
              payload = 1;
              this.makeMove(first, last, distance, end, payload);
            break;
            case "ArrowUp":
              first = 12;
              last = 0;
              distance = -4;
              end = 16;
              payload = 1;
              this.makeMove(first, last, distance, end, payload);
            break;
            case "ArrowLeft":
              first = 3;
              last = 0;
              distance = -1;
              end = 16;
              payload = 4;
              this.makeMove(first, last, distance, end, payload);
            break;
            case "ArrowRight":
              first = 0;
              last = 3;
              distance = 1;
              end = 13;
              payload = 4;
              this.makeMove(first, last, distance, end, payload);
            break;
            default:
              console.log("wrong key");
            break;
          }
          this.canGoNext = true;
        })
      }
    },
    makeMove(first, last, distance, end, payload) {

      let second, third = null;

      for(first; first < end; first += payload, last += payload) {
        second = first + distance;
        third = last - distance;
        // first is empety
        if(this.rows[first] === 0) {
          // second is empety
          if(this.rows[second] === 0) {
            // third is empety
            if(this.rows[third] === 0) {
              continue;
            } 
            // third has value
            else {
              // last is empety
              if(this.rows[last] === 0) {
                this.setValue(last, this.rows[third])
                this.setValue(third, 0);
              }
              // last has value
              else {
                if(this.rows[third] === this.rows[last] && this.rows[last] !== 0) {
                  let newValue = this.rows[third] + this.rows[last];
                  this.setValue(last, newValue);
                  this.setValue(third, 0);
                } else {
                  continue;
                }
              }
            }
          }
          // second has value
          else {
            // third is empety
            if(this.rows[third] === 0) {
              // last is empety
              if(this.rows[last] === 0) {
                this.setValue(last, this.rows[second]);
                this.setValue(second, 0);
              }
              // last has value
              else {
                if(this.rows[second] === this.rows[last] && this.rows[last] !== 0) {
                  let newValue = this.rows[second] + this.rows[last];
                  this.setValue(last, newValue);
                  this.setValue(second, 0);
                } else {
                  this.setValue(third, this.rows[second]);
                  this.setValue(second, 0);
                }
              }
            }
            // third has value
            else {
              // last is empety
              if(this.rows[last] === 0) {
                if(this.rows[second] === this.rows[third] && this.rows[third] !== 0) {
                  let newValue = this.rows[second] + this.rows[third];
                  this.setValue(last, newValue);
                  this.setValue(second, 0);
                  this.setValue(third, 0);
                } else {
                  this.setValue(last, this.rows[third]);
                  this.setValue(third, this.rows[second]);
                  this.setValue(second, 0);
                }
              }
              // last has value
              else {
                if(this.rows[last] === this.rows[third] && this.rows[last] !== 0) {
                  let newValue = this.rows[third] + this.rows[last];
                  this.setValue(last, newValue);
                  this.setValue(third, this.rows[second]);
                  this.setValue(second, 0);
                } else {
                  if(this.rows[second] === this.rows[third] && this.rows[third] !== 0) {
                    let newValue = this.rows[second] + this.rows[third];
                    this.setValue(third, newValue);
                    this.setValue(second, 0);
                  }
                }
              }
            }
          }
        } 
        // fisrt has value
        else {
          // second is empety
          if(this.rows[second] === 0) {
            // third is empety
            if(this.rows[third] === 0) {
              // last is empety
              if(this.rows[last] === 0) {
                this.setValue(last, this.rows[first]);
                this.setValue(first, 0);
              }
              // last has value
              else {
                if(this.rows[first] === this.rows[last] && this.rows[last] !== 0) {
                  let newValue = this.rows[first] + this.rows[last];
                  this.setValue(last, newValue);
                  this.setValue(first, 0);
                }
              }
            }
            // third has value
            else {
              // last is empety
              if(this.rows[last] === 0) {
                if(this.rows[first] === this.rows[third] && this.rows[third] !== 0) {
                  let newValue = this.rows[first] + this.rows[third];
                  this.setValue(last, newValue);
                  this.setValue(third, 0);
                  this.setValue(first, 0);
                } else {
                  this.setValue(last, this.rows[third]);
                  this.setValue(third, this.rows[first]);
                  this.setValue(first, 0);
                }
              }
              // last has value
              else {
                if(this.rows[third] === this.rows[last] && this.rows[last] !== 0) {
                  let newValue = this.rows[third] + this.rows[last];
                  this.setValue(last, newValue);
                  this.setValue(third, this.rows[first]);
                  this.setValue(first, 0);
                } else {
                  if(this.rows[third] === this.rows[first] && this.rows[first] !== 0) {
                    let newValue = this.rows[first] + this.rows[third];
                    this.setValue(third, newValue);
                    this.setValue(first, 0);
                  } else {
                    this.setValue(second, this.rows[first]);
                    this.setValue(first, 0);
                  }
                }
              }
            }
          }
          // second has value
          else {
            // third is empety
            if(this.rows[third] === 0) {
              // last is empety
              if(this.rows[last] === 0) {
                if(this.rows[first] === this.rows[second] && this.rows[second] !== 0) {
                  let newValue = this.rows[first] + this.rows[second];
                  this.setValue(last, newValue);
                  this.setValue(second, 0);
                  this.setValue(first, 0);
                } else {
                  this.setValue(last, this.rows[second]);
                  this.setValue(third, this.rows[first]);
                  this.setValue(second, 0);
                  this.setValue(first, 0);
                }
              }
              // last has value
              else {
                if(this.rows[last] === this.rows[second] && this.rows[second] !== 0) {
                  let newValue = this.rows[second] + this.rows[last];
                  this.setValue(last, newValue);
                  this.setValue(second, 0);
                  this.setValue(first, 0);
                } else {
                  if(this.rows[second] === this.rows[first] && this.rows[first] !== 0) {
                    let newValue = this.rows[second] + this.rows[first];
                    this.setValue(third, newValue);
                    this.setValue(second, 0);
                    this.setValue(first, 0);
                  } else {
                    this.setValue(third, this.rows[second]);
                    this.setValue(second, this.rows[first]);
                    this.setValue(first, 0);
                  }
                }
              }
            }
            // third has value
            else {
              // last is empety
              if(this.rows[last] === 0) {
                if(this.rows[third] === this.rows[second] && this.rows[second] !== 0) {
                  let newValue = this.rows[third] + this.rows[second];
                  this.setValue(last, newValue);
                  this.setValue(third, this.rows[first]);
                  this.setValue(second, 0);
                  this.setValue(first, 0);
                } else {
                  if(this.rows[second] === this.rows[first] && this.rows[first] !== 0) {
                    let newValue = this.rows[second] + this.rows[first];
                    this.setValue(last, this.rows[third]);
                    this.setValue(third, newValue);
                    this.setValue(second, 0);
                    this.setValue(first, 0);
                  } else {
                    this.setValue(last, this.rows[third]);
                    this.setValue(third, this.rows[second]);
                    this.setValue(second, this.rows[first]);
                    this.setValue(first, 0);
                  }
                }
              }
              // last has value
              else {
                if(this.rows[last] === this.rows[third] && this.rows[last] !== 0) {
                  let newValue = this.rows[last] + this.rows[third];
                  this.setValue(last, newValue);
                  if(this.rows[second] === this.rows[first] && this.rows[first] !== 0) {
                    let newValue = this.rows[second] + this.rows[first];
                    this.setValue(third, newValue);
                    this.setValue(second, 0);
                    this.setValue(first, 0);
                  } else {
                    this.setValue(third, this.rows[second]);
                    this.setValue(second, this.rows[first]);
                    this.setValue(first, 0);
                  }
                } else {
                  if(this.rows[third] === this.rows[second] && this.rows[second] !== 0) {
                    let newValue = this.rows[third] + this.rows[second];
                    this.setValue(third, newValue);
                    this.setValue(second, this.rows[first]);
                    this.setValue(first, 0);
                  } else {
                    if(this.rows[second] === this.rows[first] && this.rows[first] !== 0) {
                      let newValue = this.rows[second] + this.rows[first];
                      this.setValue(second, newValue);
                      this.setValue(first, 0);
                    }
                  }
                }
              }
            }
          }
        }
      }

    },
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Courier New', Courier, monospace;
}

.container {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.start-section, .restart-section {
  width: 100%;
  min-height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
}




/* animations */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.15s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
/* end animations */


/* bootstrap */
.btn {
  display: inline-block;
  font-weight: 400;
  color: #212529;
  text-align: center;
  vertical-align: middle;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  background-color: transparent;
  border: 1px solid transparent;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  border-radius: 0.25rem;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}
@media (prefers-reduced-motion: reduce) {
  .btn {
    transition: none;
  }
}

.btn:hover {
  color: #212529;
  text-decoration: none;
}

.btn:focus, .btn.focus {
  outline: 0;
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}

.btn.disabled, .btn:disabled {
  opacity: 0.65;
}

a.btn.disabled,
fieldset:disabled a.btn {
  pointer-events: none;
}
.btn-primary {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}

.btn-primary:hover {
  color: #fff;
  background-color: #0069d9;
  border-color: #0062cc;
}

.btn-primary:focus, .btn-primary.focus {
  box-shadow: 0 0 0 0.2rem rgba(38, 143, 255, 0.5);
}

.btn-primary.disabled, .btn-primary:disabled {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}

.btn-primary:not(:disabled):not(.disabled):active, .btn-primary:not(:disabled):not(.disabled).active,
.show > .btn-primary.dropdown-toggle {
  color: #fff;
  background-color: #0062cc;
  border-color: #005cbf;
}

.btn-primary:not(:disabled):not(.disabled):active:focus, .btn-primary:not(:disabled):not(.disabled).active:focus,
.show > .btn-primary.dropdown-toggle:focus {
  box-shadow: 0 0 0 0.2rem rgba(38, 143, 255, 0.5);
}

.btn-outline-primary {
  color: #007bff;
  border-color: #007bff;
}

.btn-outline-primary:hover {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}

.btn-outline-primary:focus, .btn-outline-primary.focus {
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.5);
}

.btn-outline-primary.disabled, .btn-outline-primary:disabled {
  color: #007bff;
  background-color: transparent;
}

.btn-outline-primary:not(:disabled):not(.disabled):active, .btn-outline-primary:not(:disabled):not(.disabled).active,
.show > .btn-outline-primary.dropdown-toggle {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}

.btn-outline-primary:not(:disabled):not(.disabled):active:focus, .btn-outline-primary:not(:disabled):not(.disabled).active:focus,
.show > .btn-outline-primary.dropdown-toggle:focus {
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.5);
}

</style>
