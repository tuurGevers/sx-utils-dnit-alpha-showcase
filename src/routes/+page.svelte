<script>
    import {Box, Typography, Navbar} from "sx-utils-dnit"
    import {api} from "../Stores";
    import {onMount} from "svelte";
    import InfoKaart from "../components/InfoKaart.svelte";

    let fetchInfo = async () => {
        let result = []
        await fetch(`${$api}/home/getinfo`).then(async (data) => result = await data.json())
        return result
    }
    onMount(() => {
        window.scrollTo(0, 0)
    })
</script>

<head>
    <title>Home - kelly verheyen</title>
    <meta name="description" content="This page contains an overview of kelly verheyen kellyverheyen.com">
    <meta name="keywords" content="homepage, home,information, kelly, vooruit , spa, politiek, ocmw, kellyverheyen.com">
</head>

<Navbar links={["home", "contact", "FAQ"]} color="orange"/>

<Box sx={{m:"5 0"}}>
    <Typography sx={{display:"none"}} variant={1}>Wie Ben Ik</Typography>
    <Box sx={{m:"4 0 0 0"}}>
        {#await fetchInfo() then infokaarten}
            {#each infokaarten as infokaart,i}
                <InfoKaart titel={infokaart.titel} tekst={infokaart.tekst} foto_pad={infokaart.foto_pad}/>
            {/each}
        {/await}
    </Box>
</Box>
