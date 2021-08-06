<template>
  <div class="add-location">
    <div class="container">
      <form>
        <div class="search" ref="search">
          <div
            class="search__main-lay outer"
            :class="{
              'with-results': searchResultsIsActive,
            }"
            @click="$refs.searchLocation.focus()"
          >
            <div class="search__icon">
              <div class="icon-wrap">
                <svg
                  focusable="false"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M15.5 14h-.79l-.28-.27A6.471 6.471 0 0 0 16 9.5 6.5 6.5 0 1 0 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"
                  ></path>
                </svg>
              </div>
            </div>
            <input
              type="search"
              class="search__input"
              v-model="searchLocation"
              ref="searchLocation"
              @focus="
                searchResultsIsActive =
                  isMinLength && searchLocationResults.length
              "
            />
            <div
              class="search__clear"
              v-if="isClearButton"
              @click="searchLocation = ''"
            >
              <div class="icon-wrap">
                <svg
                  focusable="false"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"
                  ></path>
                </svg>
              </div>
            </div>
            <div class="search__search-results" v-if="searchResultsIsActive">
              <ul class="search-results__list">
                <li
                  class="search-results__item"
                  :class="{ choosed: i === 0 }"
                  v-for="(location, i) in searchLocationResults"
                  :key="location.woeid"
                  @click="setLocation($event, location)"
                >
                  {{ location.title }}
                </li>
                <li
                  class="search-results__item"
                  v-if="searchResultsIsActive && !searchLocationResults.length"
                >
                  {{ searchResultsInfo }}
                </li>
              </ul>
            </div>
          </div>
          <!--          <button class="search__submit-btn outer" type="submit">GO</button>-->
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  name: 'AddLocationForm',
  data() {
    return {
      searchLocation: '',
      searchLocationResults: [],
      searchResultsInfo: '',
      searchResultsIsActive: false,
      disableSearchRequest: false,
    }
  },
  computed: {
    isClearButton() {
      return this.searchLocation.length
    },
    isMinLength() {
      return this.searchLocation.length >= 3
    },
  },
  methods: {
    async searchLocationRequest(val) {
      const results = await this.$axios.$get(
        `${process.env.W_SEARCH_LOCATION}?query=${val}`
      )
      this.searchLocationResults = results
      if (!results.length) {
        this.searchResultsInfo = 'Oops! Nothing to show...'
      }
    },
    setLocation(e, location) {
      this.disableSearchRequest = true
      this.searchLocation = location.title
    },
  },
  watch: {
    searchLocation(val) {
      this.searchLocationResults.splice(0)
      if (!this.isMinLength) {
        this.searchResultsIsActive = false
        return
      }
      if (this.disableSearchRequest) {
        this.searchResultsIsActive = false
        this.disableSearchRequest = false
      } else {
        this.debounceSearchLocationRequest(val)
        this.searchResultsIsActive = true
        this.searchResultsInfo = 'We are searching...'
      }
    },
  },
  created() {
    this.debounceSearchLocationRequest = _.debounce(
      this.searchLocationRequest,
      500
    )
  },
  mounted() {
    window.addEventListener('click', (e) => {
      if (!this.$refs.search.contains(e.target)) {
        this.$refs.search.blur()
        this.searchResultsIsActive = false
      }
    })
  },
}
</script>

<style scoped lang="scss">
.add-location {
  padding: 30px 0;
}
.search {
  display: inline-block;
}
.outer {
  box-shadow: none;
  border-radius: 10px;
  background: #fff;
  color: #9aa0a6;
  &:hover,
  &.focused {
    box-shadow: 0 4px 6px rgba(32, 33, 36, 0.28);
  }
}
.search__main-lay {
  display: flex;
  width: 320px;
  position: relative;
  padding: 3px 15px;
  &.with-results {
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
  }
}
.search__input {
  border: none;
  height: 34px;
  flex-grow: 1;
  -webkit-tap-highlight-color: transparent;
  -webkit-rtl-ordering: logical;
  outline: none;
}
.search__icon {
  display: flex;
  align-items: center;
  padding-right: 10px;
  pointer-events: none;
}
.search__clear {
  display: flex;
  align-items: center;
  cursor: pointer;
}
.icon-wrap {
  width: 20px;
  height: 20px;
  position: relative;
  svg {
    position: absolute;
    display: block;
    width: 100%;
    height: 100%;
    fill: currentColor;
  }
}
.search__search-results {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  background: inherit;
  border-bottom-right-radius: 10px;
  border-bottom-left-radius: 10px;
  box-shadow: 0 4px 6px rgba(32, 33, 36, 0.28);
  margin-top: -1px;
  z-index: 1;
  overflow: hidden;
  &:before {
    content: '';
    position: absolute;
    width: calc(100% - 30px);
    height: 1px;
    background: rgba(154, 160, 166, 0.28);
    left: 50%;
    transform: translateX(-50%);
  }
}
.search-results__list {
  list-style: none;
  padding: 0;
  margin: 0;
}
.search-results__item {
  padding: 15px 0 15px 45px;
  cursor: pointer;
  &:hover {
    background: #f7f8fb;
  }
  &.choosed {
    background: #e8eaea !important;
  }
}
.search__submit-btn {
  background: #4d4d51;
  min-width: 50px;
  font-size: 14px;
  font-weight: 700;
  cursor: pointer;
  margin-left: 1em;
  border: 1px solid #4d4d51;
  color: #fff;
}
</style>
