<script>
  import SearchBox from "./SearchBox.svelte";
</script>

<style lang="less">
  @import "../styles/variables";
  @import "../styles/mixins";

  .hero {
    .block-default-styles();

    height: calc(100vh - var(--topline-height));
    min-height: 42vw;
    background: url(/images/hero-bg.jpg) center/cover no-repeat;

    @media (max-width: @site-width) {
      min-height: 46vw;
    }

    @media (max-width: 1050px) {
      min-height: 530px;
    }

    @media (max-width: 830px) {
      min-height: 500px;
    }

    @media (orientation: portrait) {
      @media (max-height: 1200px) {
        height: 65vh;
      }

      @media (max-height: 900px) {
        height: 70vh;
      }

      @media (max-height: 750px) {
        height: 80vh;
      }

      @media (max-height: 675px) {
        height: calc(100vh - var(--topline-height));
      }
    }

    @media (max-width: 500px) {
      min-height: 520px;
    }

    @media (max-width: 380px + 2 * @site-paddings[small]) {
      min-height: 530px;
    }

    @media (max-width: 365px) {
      min-height: 515px;
    }

    &__container {
      .container();
    }

    &__inner {
      .block-default-styles();

      display: flex;
      flex-flow: column wrap;
      justify-content: space-between;
      align-items: flex-start;

      padding-top: 70px;
      padding-bottom: 18px;

      @media (max-width: @site-width) {
        padding-top: 50px;
      }

      @media (max-width: 830px) {
        padding-top: 30px;
      }

      @media (max-width: 500px) {
        padding-top: 10%;
        align-items: center;
      }

      @media (max-width: 380px + 2 * @site-paddings[small]) {
        padding-top: 15px;
      }
    }

    &__search-box {
      width: 100%;
      max-width: 380px;

      box-sizing: border-box;
    }

    &__tagline {
      .not-selectable();
      .body-text();

      margin: auto 0 0 auto;
      // padding: 20px 0 0 20px;
      text-align: right;

      @media (max-width: 500px) {
        margin: 0 auto;
        text-align: center;
      }
    }
  }
</style>

<div class="hero">
  <div class="hero__container">
    <div class="hero__inner">
      <div class="hero__search-box">
        <SearchBox />
      </div>
      <p class="hero__tagline">
        Лучшие номера для вашей работы,
        <br />
        отдыха и&nbsp;просто вдохновения.
      </p>
    </div>
  </div>
</div>
