<script>
  export let recipients = []

  const width = (window.innerWidth - 48) * 0.3
  const fontSize = 16
  const maxCharacters = Math.floor(width / (0.6 * fontSize))

  const shownEmails = []
  const hiddenEmails = []
  const formattedString = recipients
    .slice(1)
    .reduce((ret = '', cur = '', curIndex = -1, self = []) => {
      const hasDot = hiddenEmails.length > 0 || curIndex + 1 < self.length
      const length = ret.length + cur.length + (hasDot ? 9 : 6)
      if ((hasDot && length > maxCharacters - 3) || length > maxCharacters) {
        hiddenEmails.push(cur)
        return ret
      }
      shownEmails.push(cur)
      return ret ? ret + ', ' + cur : cur
    }, recipients?.[0] ?? '')
</script>

<style lang="scss">
  $font-color: #f0f0f0;
  $back-color: #666666;

  .recipients-cell {
    display: flex;
    justify-content: space-between;
    align-items: center;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .recipients-cell-badge {
    background-color: $back-color;
    color: $font-color;
    font-size: 16px;
    border-radius: 3px;
    padding: 2px 5px;
  }
</style>

<div class="recipients-cell">
  <span>
    {hiddenEmails.length > 0 ? formattedString + ', ...' : formattedString}
  </span>
  {#if hiddenEmails.length > 0}
    <span class="recipients-cell-badge">+{hiddenEmails.length}</span>
  {/if}

</div>
