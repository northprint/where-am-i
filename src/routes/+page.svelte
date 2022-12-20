<script lang="ts">
    import Button, { Label, Icon } from '@smui/button';
    import H from '@here/maps-api-for-javascript';

    let status: String = '';
    let location: String = '';

    function handleButtonClick() {
        const platform = new H.service.Platform({
            'apikey': ''
        });
        const service = platform.getSearchService();

        function success(position:any) {
            const latitude  = position.coords.latitude;
            const longitude = position.coords.longitude;

            status = `Latitude: ${latitude}, Longitude: ${longitude}`;

            service.reverseGeocode({
                at: `${latitude},${longitude}`,
            },
            (result: any) => {
                const locationLabel = result.items[0].address.label;
                location = `You are now in ${locationLabel}`;
            },
            (error) => {
                alert(error);
            });
        }

        function error() {
            status = 'Unable to retrieve your location';
        }

        if('geolocation' in navigator) {
            // geolocationが使用可能になっている場合の処理
            status = 'Locating…';
            navigator.geolocation.getCurrentPosition(success, error);
        } else {
            // geolocationが使用不可になっている場合の処理
            status = 'Unable to retrieve your location';
        }
    }
</script>

<Button on:click={handleButtonClick} variant="raised" class="button-shaped-round">
    <Label>Where Am I?</Label>
</Button>
<pre>{status}</pre>
<div>
    <p>{location}</p>
</div>
