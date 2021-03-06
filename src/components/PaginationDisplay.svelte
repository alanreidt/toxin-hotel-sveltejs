<script>
  export let itemsPerPage = 12;
  export let itemsInTotal = 100;
  export let currentPage = 1;
  export let maxPageQuantity = 5;
  export let pagesToDisplay = 5;

  let controlLeft;
  let controlRight;

  const pagesInTotal = Math.ceil(itemsInTotal / itemsPerPage);

  $: isFirstPage = currentPage === 1;
  $: isLastPage = currentPage === pagesInTotal;

  $: isExceededStart = currentPage >= pagesToDisplay;
  $: isExceededEnd = currentPage <= pagesInTotal - pagesToDisplay;
  $: currentPageIndex = currentPage - 1;

  const isMaxPageQuantityExceeded = pagesInTotal > maxPageQuantity;
  const pageNumbers = new Array(pagesInTotal).fill(1).map((value, index) => index + 1);

  const findDisplayedPageNumbers = (currentPageIndex, pagesToDisplay) => {
    if (isExceededStart && isExceededEnd) {
      return pageNumbers.slice(currentPageIndex - 1, currentPageIndex - 1 + 3);
    }

    if (isExceededEnd) {
      return pageNumbers.slice(0, 0 + pagesToDisplay);
    }

    if (isExceededStart) {
      return pageNumbers.slice(pagesInTotal - pagesToDisplay);
    }
  };

  $: displayedPageNumbers = isMaxPageQuantityExceeded ?
    findDisplayedPageNumbers(currentPageIndex, pagesToDisplay) :
    pageNumbers;

  function handlePaginationListClick(event) {
    const isPaginationButtonTarget = event.target.closest(".pagination-display__button") !== null;

    if (!isPaginationButtonTarget) {
      return;
    }

    if (event.target === controlRight) {
      currentPage = Math.min(pagesInTotal, currentPage + 1);
      return;
    }

    if (event.target === controlLeft) {
      currentPage = Math.max(1, currentPage - 1);
      return;
    }

    currentPage = Number(event.target.value);
  }
</script>

<style lang="less">
  @import "../styles/variables";
  @import "../styles/mixins";

  .pagination-display {
    display: block;
    width: 100%;

    &__list {
      .list-styles-reset();

      display: flex;
      flex-flow: row nowrap;
      justify-content: center;
    }

    &__item {
      display: block;
      opacity: 1;

      &_disabled {
        visibility: hidden;
        display: none;
      }
    }

    &__button {
      // reset <button>, <input> styles
      padding: 0;
      border: none;
      background-color: transparent;

      // reset <a> styles
      text-decoration: none;

      // add compatibility between html elements (<a>, <button>, <input>)
      cursor: pointer;

      // add standart behavior of a <button> to compatibility with <a>
      // isolate component's rules from external influence
      display: inline-block;
      vertical-align: middle;

      .body-text();
      .not-selectable();
      text-decoration: none;

      display: inline-block;
      vertical-align: middle;
      width: 40px;
      height: 40px;
      line-height: 40px;

      font-size: 12px;
      text-align: center;
      background-image: transparent;
      color: #colors[dark-shade-50];
      border-radius: 50%;

      &:hover,
      &:focus {
        background-color: #colors[dark-shade-25];
      }
    }

    &__button_active {
      background-image: @primary-gradient;
      color: #fff;

      &:hover,
      &:focus {
        opacity: 0.5;
      }
    }

    &__control-button {
      // .arrow();
      .decoration_type_arrow-forward();

      &_type_prev::before {
        content: "\e5c4";
      }

      &::before {
        // styles for .decoration_type_arrow-forward();
        width: inherit;
        height: inherit;
        line-height: inherit;
        color: #fff;
      }

      background-image: @secondary-gradient;

      &:hover,
      &:focus {
        opacity: 0.5;
      }
    }
  }
</style>

<div class="pagination-display">
  <ul class="pagination-display__list" on:click={handlePaginationListClick}>
    <li class="pagination-display__item {isFirstPage ? "pagination-display__item_disabled": ''}">
      <button
        bind:this={controlLeft}
        disabled={isFirstPage}
        class="pagination-display__button pagination-display__control-button pagination-display__control-button_type_prev"></button>
    </li>

    {#if (isMaxPageQuantityExceeded)}
      <li class="pagination-display__item {!isExceededStart ? "pagination-display__item_disabled": ''}">
        <button
          disabled={!isExceededStart}
          class="pagination-display__button"
          value="{pageNumbers[0]}">
          {pageNumbers[0]}
        </button>
      </li>
      <li class="pagination-display__item {!isExceededStart ? "pagination-display__item_disabled": ''}">
        <a class="pagination-display__button">...</a>
      </li>
    {/if}

    {#each displayedPageNumbers as pageNumber}
      <li class="pagination-display__item">
        <button
          class="pagination-display__button {pageNumber === currentPage ? "pagination-display__button_active" : ''}"
          value="{pageNumber}">
          {pageNumber}
        </button>
      </li>
    {/each}

    {#if (isMaxPageQuantityExceeded)}
      <li class="pagination-display__item {!isExceededEnd ? "pagination-display__item_disabled": ''}">
        <a class="pagination-display__button">...</a>
      </li>
      <li class="pagination-display__item {!isExceededEnd ? "pagination-display__item_disabled": ''}">
        <button
          disabled={!isExceededEnd}
          class="pagination-display__button"
          value="{pageNumbers[pageNumbers.length - 1]}">
          {pageNumbers[pageNumbers.length - 1]}
        </button>
      </li>
    {/if}

    <li class="pagination-display__item {isLastPage ? "pagination-display__item_disabled": ''}">
      <button
        bind:this={controlRight}
        disabled={isLastPage}
        class="pagination-display__button pagination-display__control-button"></button>
    </li>
  </ul>
</div>
