<script>
  export let titles = ["Gorgeous", "Good", "Normal", "Bad", "Terrible"];
  export let id;
  export let rating;

  rating = Number(rating);

  const hasRating = Number.isFinite(rating);

  let values = [];

  titles.forEach((title, index) => {
    values.push(titles.length - index);
  });
</script>

<style lang="less">
  @import "../styles/variables";
  @import "../styles/mixins";

  .rating {
    display: flex;
    flex-direction: row-reverse;
    justify-content: flex-end;

    &__label {
      .visually-hidden();
    }

    &__input {
      .default-styles-reset();
      margin: 0;

      display: inline-block;
      vertical-align: middle;

      &_disabled {
        pointer-events: none;
      }

      &:hover::before {
        content: "\e838";
        color: darken(#colors[primary], 5%);
      }

      &:hover ~ &::before {
        content: "\e838";
        color: darken(#colors[primary], 5%);
      }

      &:checked::before {
        content: "\e838";
      }

      &:checked ~ &::before {
        content: "\e838";
      }
    }

    &__input::before {
      .material-icons();

      content: "\e83a";
      display: inline-block;
      vertical-align: middle;

      font-size: 24px;
      color: #colors[primary];

      cursor: pointer;
    }
  }
</style>

<div class="rating">
  {#each titles as title, index}
    <input
      class="rating__input {hasRating ? "rating__input_disabled" : ''}"
      type="radio"
      id={id + '-' + values[index]}
      value={values[index]}
      checked={values[index] === rating}
      disabled={hasRating}
      {title}
      {...$$restProps} />
    <label class="rating__label" for={id + '-' + values[index]}>
      {values[index] + ' star'}
    </label>
  {/each}
</div>
