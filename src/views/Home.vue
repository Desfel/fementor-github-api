<template>
  <div class="page-wrapper" :class="{'theme-dark': isDark}">
    <div class="app-wrapper">
      <section class="header-section">
        <h1>devfinder</h1>

        <p class="theme-toggle" v-if="!isDark" @click="themeToggle">
          Dark
          <svg width="20" height="20" xmlns="http://www.w3.org/2000/svg"><path d="M19.513 11.397a.701.701 0 00-.588.128 7.496 7.496 0 01-2.276 1.336 7.101 7.101 0 01-2.583.462 7.505 7.505 0 01-5.32-2.209 7.568 7.568 0 01-2.199-5.342c0-.873.154-1.72.41-2.49a6.904 6.904 0 011.227-2.21.657.657 0 00-.102-.924.701.701 0 00-.589-.128C5.32.61 3.427 1.92 2.072 3.666A10.158 10.158 0 000 9.83c0 2.8 1.125 5.342 2.967 7.19a10.025 10.025 0 007.16 2.98c2.353 0 4.527-.822 6.266-2.183a10.13 10.13 0 003.58-5.624.623.623 0 00-.46-.796z" fill="#697C9A" fill-rule="nonzero"/></svg>
        </p>

        <p class="theme-toggle" v-if="isDark" @click="themeToggle">
          Light
          <svg width="20" height="20" xmlns="http://www.w3.org/2000/svg"><g fill="#FFF" fill-rule="nonzero"><path d="M13.545 6.455c-.9-.9-2.17-1.481-3.545-1.481a4.934 4.934 0 00-3.545 1.481c-.9.9-1.481 2.17-1.481 3.545 0 1.376.582 2.646 1.481 3.545.9.9 2.17 1.481 3.545 1.481a4.934 4.934 0 003.545-1.481c.9-.9 1.481-2.17 1.481-3.545a4.934 4.934 0 00-1.481-3.545zM10 3.413a.7.7 0 00.688-.688V.688A.7.7 0 0010 0a.7.7 0 00-.688.688v2.037a.7.7 0 00.688.688zM15.635 5.344l1.455-1.455a.67.67 0 000-.952.67.67 0 00-.952 0l-1.455 1.455a.67.67 0 000 .952c.238.264.66.264.952 0zM19.312 9.312h-2.037a.7.7 0 00-.688.688.7.7 0 00.688.688h2.037A.7.7 0 0020 10a.7.7 0 00-.688-.688zM15.608 14.656a.67.67 0 00-.952 0 .67.67 0 000 .952l1.455 1.455a.67.67 0 00.952 0 .67.67 0 000-.952l-1.455-1.455zM10 16.587a.7.7 0 00-.688.688v2.037A.7.7 0 0010 20a.7.7 0 00.688-.688v-2.037a.7.7 0 00-.688-.688zM4.365 14.656L2.91 16.111a.67.67 0 000 .952.67.67 0 00.952 0l1.455-1.455a.67.67 0 000-.952c-.238-.264-.66-.264-.952 0zM3.413 10a.7.7 0 00-.688-.688H.688A.7.7 0 000 10a.7.7 0 00.688.688h2.037A.7.7 0 003.413 10zM4.365 5.344a.67.67 0 00.952 0 .67.67 0 000-.952L3.862 2.937a.67.67 0 00-.952 0 .67.67 0 000 .952l1.455 1.455z"/></g></svg>        </p>
      </section>

      <section class="form-wrapper">
        <form action="#" method="POST" @submit.prevent="searchUser">
          <img src="@/assets/img/icon-search.svg" alt="Search" />
          <input v-model="username" type="text" name="username" placeholder="Search GitHub username…" />

          <p class="error-message" :class="{'is-visible' : this.errors.length > 0}">No results</p>
          <button type="submit" class="submit-btn">Search</button>
        </form>
      </section>

      <section class="card-wrapper">
        <img class="avatar desktop-avatar" :src="userInfo.avatar_url" :alt="userInfo.login" />

        <div class="card-contents">

          <div class="card-header">
            <img class="avatar mobile-avatar" :src="userInfo.avatar_url" :alt="userInfo.login" />

            <div class="user-text">
              <div class="username-info">
                <p class="username" v-text="userInfo.name">The Octocat</p>
                <p class="github-username" v-text="`@${userInfo.login}`">@octocat</p>
              </div>
              <p class="joined-date" v-text="`Joined ${userInfo.created_at}`">Joined 25 Jan 2011</p>
            </div>

          </div>

          <p class="user-bio" v-text="userInfo.bio">
            Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Donec odio.
            Quisque volutpat mattis eros.
          </p>

          <div class="user-repo-wrapper">
            <div class="repo-block">
              <p class="repo-title">Repos</p>
              <p class="repo-count" v-text="userInfo.public_repos">8</p>
            </div>
            <div class="repo-block">
              <p class="repo-title">Followers</p>
              <p class="repo-count" v-text="userInfo.followers">3938</p>
            </div>
            <div class="repo-block">
              <p class="repo-title">Following</p>
              <p class="repo-count" v-text="userInfo.following">9</p>
            </div>
          </div>

          <div class="social-wrapper">
            <div class="social-columns">
              <div class="social-block" :class="{'is-na': this.userInfo.location === 'na'}">
                <svg height="20" width="14" xmlns="http://www.w3.org/2000/svg"><path d="M12.797 3.425C11.584 1.33 9.427.05 7.03.002a7.483 7.483 0 00-.308 0C4.325.05 2.17 1.33.955 3.425a6.963 6.963 0 00-.09 6.88l4.959 9.077.007.012c.218.38.609.606 1.045.606.437 0 .828-.226 1.046-.606l.007-.012 4.96-9.077a6.963 6.963 0 00-.092-6.88zm-5.92 5.638c-1.552 0-2.813-1.262-2.813-2.813s1.261-2.812 2.812-2.812S9.69 4.699 9.69 6.25 8.427 9.063 6.876 9.063z" fill="#4b6a9b"/></svg>
                <p v-text="`${userInfo.location === 'na' ? 'Not available' : userInfo.location}`">San Francisco</p>
              </div>
              <div class="social-block" :class="{'is-na': this.userInfo.blog === 'na'}">
                <svg height="20" width="20" xmlns="http://www.w3.org/2000/svg"><g fill="#4b6a9b"><path d="M7.404 5.012c-2.355 2.437-1.841 6.482.857 8.273.089.06.207.048.283-.027.568-.555 1.049-1.093 1.47-1.776a.213.213 0 00-.084-.3A2.743 2.743 0 018.878 10.1a2.64 2.64 0 01-.223-1.803c.168-.815 1.043-1.573 1.711-2.274l-.004-.002 2.504-2.555a2.568 2.568 0 013.648-.019 2.6 2.6 0 01.037 3.666l-1.517 1.56a.266.266 0 00-.06.273c.35 1.012.435 2.44.201 3.519-.006.03.031.05.053.028l3.228-3.295c2.062-2.105 2.044-5.531-.04-7.615a5.416 5.416 0 00-7.691.04L7.417 4.998l-.013.014z"/><path d="M13.439 13.75a.401.401 0 00.006-.003c.659-1.204.788-2.586.48-3.933l-.002.002-.001-.001a5.434 5.434 0 00-2.19-3.124.3.3 0 00-.333.015c-.553.448-1.095 1.021-1.452 1.754a.243.243 0 00.096.317c.415.24.79.593 1.04 1.061h.001c.196.33.388.958.263 1.632-.116.894-1.019 1.714-1.736 2.453-.546.559-1.935 1.974-2.49 2.542a2.6 2.6 0 01-3.666.037 2.6 2.6 0 01-.038-3.666l1.521-1.564A.266.266 0 005 11.004c-.338-1.036-.43-2.432-.217-3.51.006-.03-.031-.049-.053-.027l-3.179 3.245c-2.083 2.126-2.066 5.588.04 7.693 2.125 2.083 5.57 2.048 7.653-.078.723-.81 3.821-3.678 4.195-4.577z"/></g></svg>
                <a :href="`${userInfo.blog === 'na' ? '#' : userInfo.blog}`" v-text="`${userInfo.blog === 'na' ? 'Not available' : userInfo.blog}`" target="_blank">https://github.blog</a>
              </div>
            </div>
            <div class="social-columns">
              <div class="social-block" :class="{'is-na': this.userInfo.twitter_username === 'na'}">
                <svg height="18" width="20" xmlns="http://www.w3.org/2000/svg"><path d="M20 2.799a8.549 8.549 0 01-2.363.647 4.077 4.077 0 001.804-2.266 8.194 8.194 0 01-2.6.993A4.099 4.099 0 009.75 4.977c0 .324.027.637.095.934-3.409-.166-6.425-1.8-8.452-4.288a4.128 4.128 0 00-.56 2.072c0 1.42.73 2.679 1.82 3.408A4.05 4.05 0 01.8 6.598v.045a4.119 4.119 0 003.285 4.028 4.092 4.092 0 01-1.075.135c-.263 0-.528-.015-.776-.07.531 1.624 2.038 2.818 3.831 2.857A8.239 8.239 0 01.981 15.34 7.68 7.68 0 010 15.285a11.543 11.543 0 006.29 1.84c7.545 0 11.67-6.25 11.67-11.667 0-.182-.006-.357-.015-.53A8.18 8.18 0 0020 2.798z" fill="#4b6a9b"/></svg>
                <a v-if="this.userInfo.twitter_username !== 'na'" :href="`${'https://twitter.com/' + userInfo.twitter_username}`" target="_blank">
                  @{{ userInfo.twitter_username }}
                </a>
                <p v-else>Not available</p>
              </div>
              <div class="social-block" :class="{'is-na': this.userInfo.company === 'na'}">
                <svg height="20" width="20" xmlns="http://www.w3.org/2000/svg"><g fill="#4b6a9b"><path d="M10.858 1.558L1.7.167A1.477 1.477 0 00.517.492 1.49 1.49 0 000 1.608v17.559c0 .458.375.833.833.833h2.709v-4.375c0-.808.65-1.458 1.458-1.458h2.083c.809 0 1.459.65 1.459 1.458V20h3.541V3a1.46 1.46 0 00-1.225-1.442zM4.583 12.292h-1.25a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zm0-2.5h-1.25a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zm0-2.5h-1.25a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zm0-2.5h-1.25a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zm4.167 7.5H7.5a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zm0-2.5H7.5a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zm0-2.5H7.5a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zm0-2.5H7.5a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zM18.85 9.035l-5.933-1.242V20h5.625A1.46 1.46 0 0020 18.542V10.46c0-.688-.47-1.274-1.15-1.425zM16.875 17.5h-1.25a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zm0-2.5h-1.25a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25zm0-2.5h-1.25a.625.625 0 010-1.25h1.25a.625.625 0 010 1.25z"/></g></svg>
                <p v-text="`${userInfo.company === 'na' ? 'Not available' : userInfo.company}`">@github</p>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import axios from 'axios'
import moment from 'moment'

export default {
  data() {
    return {
      isDark: false,
      userInfo: {
        avatar_url: '../img/octocat.png',
        name: 'The Octocat',
        login: 'octocat',
        created_at: '25 Jan 2011',
        bio:
          'Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Donec odio. Quisque volutpat mattis eros.',
        public_repos: 8,
        followers: 3938,
        following: 9,
        location: 'San Francisco',
        blog: 'https://github.blog',
        twitter_username: 'na',
        company: '@github',
      },
      username: '',
      errors: []
    }
  },
  head() {
    return {
      title: {
        inner: 'Home'
      },
      meta: [
        {
          name: 'description',
          content: `${this.appTitle} home page`,
          id: 'desc'
        }
      ]
    }
  },
  methods: {
    themeToggle() {
      this.isDark = !this.isDark
    },
    searchUser() {
      const trimmedUser = this.username.trim()
      if (trimmedUser !== '') {
        axios.get(`https://api.github.com/users/${this.username}`)
          .then(response => {
            this.errors = []

            this.userInfo = response.data
            this.userInfo.created_at = moment(this.userInfo.created_at).format('D MMM YYYY')

            const userLocation = this.userInfo.location === null ? '' : this.userInfo.location.trim()
            this.userInfo.location = userLocation === '' ? 'na' : userLocation

            const userBlog = this.userInfo.blog === null ? '' : this.userInfo.blog.trim()
            this.userInfo.blog = userBlog === '' ? 'na' : userBlog

            const userTwitter = this.userInfo.twitter_username === null ? '' : this.userInfo.twitter_username.trim()
            this.userInfo.twitter_username = userTwitter === '' ? 'na' : userTwitter

            const userCompany = this.userInfo.company === null ? '' : this.userInfo.company.trim()
            this.userInfo.company = userCompany === '' ? 'na' : userCompany
          })
          .catch(e => {
            this.errors.push(e)
          })
      }
    },
  },
  computed: mapState('app', ['appTitle'])
}
</script>

<style lang="scss" scoped>
@import '@/theme/variables.scss';

.page-wrapper {
  display: flex;
  flex-direction: column;
  background: var(--bgColor);
  padding: 144px 10%;

  @media (max-width: 767px) {
    padding: 32px 24px;
  }

  &.theme-dark {
    form,
    .card-wrapper {
      box-shadow: none !important;
    }

    .header-section {
      .theme-toggle {
        &:hover {
          color: #90A4D4;

          svg path {
            fill: #90A4D4;
          }
        }
      }
    }
  }

  .app-wrapper {
    width: 100%;
    max-width: 730px;
    margin: auto;
  }

  .header-section {
    display: flex;
    align-items: center;
    margin-bottom: 36px;

    .theme-toggle {
      display: flex;
      align-items: center;
      margin-left: auto;
      font-size: 13px;
      font-weight: bold;
      line-height: 19px;
      letter-spacing: 2.5px;
      text-transform: uppercase;
      color: var(--textColor);
      cursor: pointer;

      &:hover {
        color: var(--titleColor);

        svg path {
          fill: var(--titleColor);
        }
      }

      svg {
        margin-left: 16px;
      }
    }
  }

  .form-wrapper {
    width: 100%;
    margin-bottom: 24px;

    form {
      display: flex;
      align-items: center;
      padding: 9.5px 10px 9.5px 32px;
      box-shadow: 0 16px 30px -10px rgba(70, 96, 187, 0.198567);
      border-radius: 15px;
      background: var(--bgContainer);

      @media (max-width: 767px) {
        padding: 6.5px 7px 7.5px 16px;
      }

      input {
        font-family: $textFont;
        margin-left: 24px;
        border: none;
        outline: none;
        font-weight: normal;
        font-size: 18px;
        line-height: 25px;
        color: var(--titleColor);
        flex-grow: 1;
        background: transparent;
        text-overflow: ellipsis;

        @media (max-width: 767px) {
          margin-left: 9px;
          margin-right: 7px;
          font-size: 13px;
          max-width: calc(100% - 140px);
        }

        &::placeholder {
          color: var(--textColor);
          text-overflow: ellipsis;
        }
      }

      .error-message {
        display: none;
        font-weight: bold;
        font-size: 15px;
        line-height: 22px;
        color: $primaryColor2;
        margin-right: 24px;

        &.is-visible {
          display: block;
        }
      }

      .submit-btn {
        padding: 12.5px 24px 13.5px;
        font-family: $textFont;
        font-weight: bold;
        font-size: 16px;
        line-height: 24px;
        color: #fff;
        border-radius: 10px;
        background: $primaryColor1;
        outline: none;
        border: none;
        cursor: pointer;

        @media (max-width: 767px) {
          margin-left: auto;
          font-size: 14px;
          line-height: 21px;
          padding: 12.5px 14px 12.5px 18px;
        }

        &:hover {
          background: lighten($primaryColor1, 19%);
        }
      }
    }
  }

  .card-wrapper {
    display: flex;
    align-items: flex-start;
    padding: 48px;
    background: var(--bgContainer);
    box-shadow: 0 16px 30px -10px rgba(70, 96, 187, 0.198567);
    border-radius: 15px;

    @media (max-width: 1024px) {
      padding: 40px;
    }

    @media (max-width: 767px) {
      padding: 32px 24px;
    }


    .desktop-avatar {
      @media (max-width: 1024px) {
        display: none;
      }
    }

    .avatar {
      width: 117px;
      height: 117px;
      border-radius: 50%;

      @media (max-width: 767px) {
        width: 70px;
        height: 70px;
      }
    }

    .card-contents {
      display: flex;
      flex-direction: column;
      margin-left: 37px;
      width: 100%;

      @media (max-width: 1024px) {
        margin: 0;
      }
    }

    .card-header {
      display: flex;
      margin-bottom: 20px;

      .mobile-avatar {
        display: none;

        @media(max-width: 1024px) {
          display: block;
          margin-right: 24px;
        }
      }

      .user-text {
        width: 100%;
        display: flex;

        @media (max-width: 1024px) {
          flex-direction: column;
        }
      }

      .username-info {
        .username {
          margin-bottom: 2px;
          font-weight: bold;
          font-size: 26px;
          line-height: 39px;
          color: var(--titleColor);

          @media (max-width: 767px) {
            font-size: 16px;
            line-height: 24px;
          }
        }

        .github-username {
          font-weight: normal;
          font-size: 16px;
          line-height: 24px;
          color: $primaryColor1;

          @media (max-width: 767px) {
            font-size: 13px;
            line-height: 19px;
          }
        }
      }

      .joined-date {
        margin-top: 8px;
        margin-left: auto;
        font-weight: normal;
        font-size: 15px;
        line-height: 22px;
        color: var(--textColor);

        @media(max-width: 1024px) {
          margin-left: 0;
          margin-top: 4px;
        }

        @media (max-width: 767px) {
          font-size: 13px;
          line-height: 19px;
        }
      }
    }

    .user-bio {
      font-style: normal;
      font-weight: normal;
      font-size: 15px;
      line-height: 25px;
      color: var(--textColor);

      @media (max-width: 767px) {
        font-size: 13px;
      }
    }

    .user-repo-wrapper {
      display: flex;
      justify-content: space-between;
      margin: 32px 0 37px;
      padding: 15px 5.188rem 17px 32px;
      border-radius: 10px;
      background: var(--bgColor);

      @media (max-width: 1024px) {
        padding-right: 2.5rem;
      }

      @media (max-width: 767px) {
        text-align: center;
        padding: 18px 15px;
        margin: 23px 0;
      }

      .repo-title {
        font-weight: normal;
        font-size: 13px;
        line-height: 19px;
        color: var(--textColor);

        @media (max-width: 767px) {
          font-size: 11px;
          line-height: 16px;
        }
      }

      .repo-count {
        font-weight: bold;
        font-size: 22px;
        line-height: 33px;
        text-transform: uppercase;
        color: var(--titleColor);

        @media (max-width: 767px) {
          margin-top: 8px;
          font-size: 16px;
          line-height: 24px;
        }
      }
    }

    .social-wrapper {
      display: flex;
      align-items: stretch;
      margin-bottom: -21px;

      @media (max-width: 767px) {
        flex-direction: column;
        margin-bottom: -16px;
      }

      .social-columns {
        &:first-child {
          margin-right: 3.75rem;
        }
      }

      .social-block {
        display: flex;
        align-items: center;
        margin-bottom: 21px;

        @media (max-width: 767px) {
          margin-bottom: 16px;
        }

        &.is-na {
          opacity: .5;
          pointer-events: none;
        }

        p, a {
          font-weight: normal;
          font-size: 15px;
          line-height: 22px;
          margin-left: 20px;
          color: var(--textColor);
          max-width: 210px;
          word-wrap: break-word;

          @media (max-width: 767px) {
            font-size: 13px;
            line-height: 19px;
          }
        }

        a {
          &:hover {
            text-decoration: underline;
          }
        }

        svg path {
          fill: var(--textColor);
        }
      }
    }
  }
}
</style>
