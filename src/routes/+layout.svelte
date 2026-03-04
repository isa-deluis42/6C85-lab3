<script>
    import { base } from "$app/paths";
    import { page } from "$app/stores";

    let pages = [
        {url: "/", title: "About"},
        {url: "/projects", title: "Projects"},
        {url: "/resume", title: "Resume"},
        {url: "/contact", title: "Contact"},
        {url: "https://github.com/isa-deluis42", title: "Github"},
        // add other pages here
    ];
    let colorScheme = "light dark";
    let localStorage = globalThis.localStorage ?? {};

    if (localStorage.colorScheme) { // if localStorage has a colorScheme property
        colorScheme = localStorage.colorScheme; // override the default colorScheme
    }
    $: localStorage.colorScheme = colorScheme;
    
    let root = globalThis.document?.documentElement;
    $: root?.style.setProperty("color-scheme", colorScheme);


</script>
<label class="color-scheme-switch">
    Theme:
    <select bind:value={ colorScheme }>
        <option value="light dark">Automatic</option>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
    </select>
</label>
<nav class = "menu">
    <ul>
        <!-- <li> <a href = "." class = "current">Home</a></li>
        <li> <a href = "projects">Projects</a></li>
        <li> <a href = "contact">Contact</a></li>
        <li> <a href = "resume"> CV </a></li>
        <li> <a href = "https://github.com/isa-deluis42" target="_blank">Github</a></li> -->
        {#each pages as p}
            <li> <a href = {p.url.startsWith("http") ? p.url : base + p.url}
            class:current={p.url === "/" // is this link the home page?
            ? $page.url.pathname === (base + "/") // if yes - set current = true if the path name matches. Else, set current = true if the path name starts correctly
            : $page.url.pathname.startsWith(base + p.url)}
            target={p.url.startsWith("http") ? "_blank" : null}>{p.title}</a></li>
        {/each}

    </ul>
    
</nav>
<style>
    :root {
    --color-accent: oklch(65% 50% 0);
    color-scheme: dark;
    }
    nav {
        --border-color: oklch(50% 10% 200 / 40%)
        margin-bottom: 0.15em;
        border-bottom-width: .1em;
        border-bottom-style: solid;
        border-bottom-color: 2px solid var(--border-color);
    }
  
    ul {
        display: contents;
        display: flex;
    }

    li {
        display: contents;
    }

    a {
        flex : 1;
        text-align: center;
        text-decoration: none;
        color: inherit;
        
        padding: 0.1em;
    }

    a.current{
        border-bottom-width: 0.4em;
        border-bottom-style: solid;
        margin-bottom: -1.25em;
        border-bottom: 2px solid var(--border-color); 
    }

    a:hover {
        border-bottom-width: 0.4em;
        border-bottom-style: solid;
        padding-bottom: 0em;
        margin-bottom: -1.25em;
        border-bottom-color: var(--color-accent);
        background-color: color-mix(in oklch, var(--color-accent), canvas 85%);
    }
    label.color-scheme-switch {
        position: absolute;
        top: 1rem;
        right: 1rem;
        display: inline-flex;
        gap: 0.5rem;
        font-size: 80%;
    }
</style>
<slot /> 