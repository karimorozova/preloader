<template>
    <div>
        <div>
            <ul class="list"> 
                <li class="list-item" v-for="(item, index) in filteredList" :data-item="item" @click="event => addElement(event, item, index)" :key="item"> {{ item }} </li>
                
            </ul>
            <div class="total-count">{{ list.length }}</div>
        </div>
        <div class="listbox-select buttons">
            <div class="buttons-button">
                <button @clicked="moveSome" :is-disabled="chosenListByKey.length < 1" >Move Some</button>
            </div>
            <div class="buttons-button">
                <button @clicked="removeSome" :is-disabled="chosenListByKey.length < 1" >Remove Some</button>
            </div>
            <div class="buttons-button">
                <button @clicked="moveAll">Move All</button>
            </div>
            <div class="buttons-button">
                <button @clicked="removeAll">Remove All</button>
            </div>
        </div>
        <div>
            <ul class="list"> 
                <li class="list-item" v-for="(item, index) in filteredSelectedList" :data-item="item" @click="event => addElement(event, item, index)" :key="item"> {{ item }} </li>
            </ul>
            <div class="select-count"> Selected: {{ mutableSelectedList.length }}</div>
        </div>
        
    </div>

</template>

<script>

export default {
  name: 'DualListbox',
  props: {
    title: {
      type: String,
      default: ''
    },
    height: {
      type: String,
      default: '180px'
    },
    width: {
      type: String,
      default: '220px'
    },
    list: {
      type: Array,
      default: () => ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
    },
    selectedList: {
      type: Array,
      default: () => []
    },
    isDraggable: {
      type: Boolean,
      default: false
    },
    isCheck: {
      type: Boolean,
      default: false
    },
    checkedList: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      isSearchBox: false,
      searchValue: '',
      isSearchBoxChosen: false,
      searchChosenValue: '',
      isFilter: false,
      mutableSelectedList: [],
      chosenListByKey: [],
      indexes: []
    }
  },
  computed: {
    filteredList() {
      let result = this.list
      if (this.searchValue) return result.filter(item => item.toLowerCase().indexOf(this.searchValue.toLowerCase()) !== -1)
      return result
    },
    filteredSelectedList() {

      this.mutableSelectedList = JSON.parse(JSON.stringify(this.selectedList))
      this.mutableSelectedList = [...new Set(this.mutableSelectedList)]
      let result = this.mutableSelectedList
      if (this.searchChosenValue) return result.filter(item => item.toLowerCase().indexOf(this.searchChosenValue.toLowerCase()) !== -1)

      return result
    },
    hasFilterSlot() {
      return !!this.$slots.filter;
    },

  },
  methods: {
    addElement(ev, el, index) {
      if (ev.target.hasAttribute('data-item') && ev.ctrlKey) {
        ev.target.classList.toggle('hover')
        this.chosenListByKey.push(el)
        if (!ev.target.classList.contains('hover')) {
          this.chosenListByKey = this.chosenListByKey.filter(item => item !== el)
        }
        return
      }
      if (ev.target.hasAttribute('data-item') && ev.shiftKey) {
        ev.target.classList.toggle('hover')
        this.indexes.push(index)
        this.indexes = this.indexes.sort()
        for (let i = this.indexes[0]; i <= this.indexes[this.indexes.length - 1]; i += 1) {
          this.chosenListByKey.push(this.filteredList[i])
          this.addClass(this.chosenListByKey)
        }
        return
      }
      const elements = document.querySelectorAll('.hover')
      for (const element of elements) {
        element.classList.remove('hover')
      }
      this.chosenListByKey = []
      this.mutableSelectedList.push(this.filteredList[index])
      this.$emit('toggleSelected', this.mutableSelectedList)
    },
    removeElement(ev, el, index) {
      console.log(55)
      if (ev.target.hasAttribute('data-item') && ev.ctrlKey) {
        ev.target.classList.toggle('hover')
        this.chosenListByKey.push(el)
        if (!ev.target.classList.contains('hover')) {
          this.chosenListByKey = this.chosenListByKey.filter(item => item !== el)
        }
        return
      }
      if (ev.target.hasAttribute('data-item') && ev.shiftKey) {
        ev.target.classList.toggle('hover')
        this.indexes.push(index)
        this.indexes = this.indexes.sort()
        for (let i = this.indexes[0]; i <= this.indexes[this.indexes.length]; i += 1) {
          this.chosenListByKey.push(this.filteredSelectedList[i])
          this.addClass(this.chosenListByKey)
        }
        return
      }
      this.mutableSelectedList.splice(index, 1)
      this.$emit('toggleSelected', this.mutableSelectedList)
    },
    moveSome() {
      this.mutableSelectedList = [...this.mutableSelectedList, ...this.chosenListByKey]
      for (let i = 0; i < this.chosenListByKey.length; i += 1) {
        const elements = document.getElementsByClassName('hover')
        for (const element of elements) {
          element.classList.remove('hover')
        }
      }
      this.$emit('toggleSelected', this.mutableSelectedList)
      this.chosenListByKey = []
      this.indexes = []
    },
    removeSome() {
      for (let i = 0; i < this.chosenListByKey.length; i += 1) {
        this.mutableSelectedList = this.mutableSelectedList.filter(item => item !== this.chosenListByKey[i])
        const elements = document.getElementsByClassName('hover')
        for (const element of elements) {
          element.classList.remove('hover')
        }
      }
      this.$emit('toggleSelected', this.mutableSelectedList)
      this.chosenListByKey = []
      this.indexes = []
    },
    moveAll() {
      this.mutableSelectedList = [...this.mutableSelectedList, ...this.list]
      this.$emit('toggleSelected', this.mutableSelectedList)
    },
    removeAll() {
      this.mutableSelectedList = []
      this.$emit('toggleSelected', this.mutableSelectedList)
    },
    addClass(arr) {
      const elements = document.querySelectorAll('[data-item]')
      for (let i = 0; i < elements.length; i += 1) {
        arr.map(item => {
          if (item === elements[i].textContent) {
            elements[i].classList.add('hover')
          }
        })
      }
    },
    toggleShowBox(prop, value) {
      this[prop] = !this[prop]
      this[value] = ''
    },
    clearFilter(value) {
      this[value] = ''
    },
    changeItemPosition(sortedArr) {
      this.mutableSelectedList = sortedArr
      this.$emit('toggleSelected', this.mutableSelectedList)
    },
    checkItem(item, index) {
      this.$emit('check', {name: item, index})
    },
  },
  beforeUnmount() {
    this.$el.removeEventListener('click', (evt) => {
      this.addElement(evt)
    })
    this.$el.removeEventListener('click', (evt) => {
      this.removeElement(evt)
    })
  },
}
</script>

<style scoped>

.dual {
  width: fit-content;
}

.listbox-title {
    margin-bottom: 10px;
    position: relative;
  } 

.listbox-select-wrapper {
    gap: 10px;
  }

.buttons {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.listbox-titles {
  margin-top: 20px;
  height: 10px;
}

.select-count {
  opacity: .4;
  float: right;
  font-size: 14px;
  margin-top: 20px;
}

.total-count {
  opacity: .4;
  float: left;
  font-size: 14px;
  margin-top: 20px;
}

.list {
  box-sizing: border-box;
  border: 1px solid gray;
  border-radius: 2px;
  overflow-y: auto;
}
.list-input {
    width: 100%;
    position: relative;
  }
.list-item {
    cursor: pointer;
    padding: 5px 7px;
    height: 28px;
    border-bottom: 1px solid #ededed;
    display: flex;
    align-items: center;
    transition: .1s ease-out;
    justify-content: space-between;
    position: relative;
    user-select: none;
  }
.list-item:hover {
      background-color: gray;
    }
 .item {
      flex-grow: 1;
    }
input {
    background: gray;
    box-sizing: border-box;
    width: 100%;
    padding: 0 7px;
    outline: none;
    border: none;
    height: 32px;
    color: #333333;
    border-bottom: 1px solid gray;
  }
i {
  margin-top: 1px;
}

</style>
