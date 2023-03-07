<script>
    import {api} from "../Stores";
    import {Box, Typography, Image, P, Card, Fly} from "sx-utils-dnit";

    let isInView;
    export let titel, foto_pad, tekst
    import {inview} from 'svelte-inview';


    let fetchImage = async () => {
        let response;
        await fetch(`${$api}/home/getimage`, {
            method: "POST",
            body: JSON.stringify({
                path: foto_pad
            }),
            headers: {
                'Content-Type': 'application/json',
            },
        }).then((res) => res.blob()).then((blob) => response = URL.createObjectURL(blob)).then(() => console.log(response))
        return response
    }
</script>

{#await fetchImage() }
    <p>loading</p>
{:then image}
    <div use:inview={{ unobserveOnEnter: true, rootMargin: '-8%' }}
         on:change={({ detail }) => {
                            isInView = detail.inView}}>
    </div>
    <Card sx={{border:"2px solid #333",backgroundColor: "#f9f9f9", p:3, width:"90%", m:"3 auto 0 auto", minHeight:"75vh"}}
          float={1} transition>
        {#if isInView}
            <Fly y={200} duration={2000}>
                <Typography sx={{fontSize:"32px", m:"0 0 3 0"}} align="center" variant={2}
                >{titel}</Typography>
            </Fly>

            <Fly x={200} duration={2000}>
                <Box sx={{flex:{sm:"col", lg:"row"}, justifyContent:"center", alignItems:"center", m:"auto 0"}}>
                    <Image src={image}
                           sx={{width:{sm:"100%",lg:"70%"}, height:"auto", maxWidth:"500px" ,rounding:1, p:"0 4% 0 0" }}
                           alt="foto"/>
                    <P sx={{width:{sm:"80%",lg:"40%"}, fontSize: "18px", lineHeight:"1.5"}} align="justify">{@html tekst}</P>
                </Box>
            </Fly>
        {/if}
    </Card>
{/await}
