
<div id="content">
    <slot></slot>
</div>

<style>
    :global(body) {
        overflow: hidden;
    }
    
    #content {
        position: fixed;
        top: 0;
        left: 0;
        height: 100vh;
        width: 100vw;
        overflow: hidden;
    }
</style>