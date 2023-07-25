<script>
    import Katex from 'svelte-katex'
</script>
 

<p>To open the commitment, someone requires the following list:</p>
<Katex displayMode>
    c, PrivKey_{'{builder}'},address_{'{searcher}'}
</Katex>

<p>The builder can subsequently verify the commitment as follows:</p>

<Katex displayMode>
true/false := verify(c, Privkey_{'{builder}'}, address_{'{searcher}'})
</Katex>
