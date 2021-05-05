<template>
    <Page class="page">
        <ActionBar class="action-bar">
            <Label class="action-bar-title"></Label>
        </ActionBar>
        <StackLayout>
            <StackLayout>
                <TextField v-model="user.email"></TextField>
            </StackLayout>
            <StackLayout>
                <TextField v-model="user.password" secure="true"></TextField>
            </StackLayout>
            <StackLayout>
                <Button text="Login" @tap="onLogin"></Button>
            </StackLayout>
        </StackLayout>
    </Page>
</template>

<script>
    import App from "./App";
    import {TnsEcho} from 'nativescript-laravel-echo';
    import * as http from 'tns-core-modules/http';

    const headers = {
        'Accept': 'application/json',
        'Content-type': 'application/json',
    };
    const apiUrl = 'https://8576f6109e94.ngrok.io';
    const socketUrlD = 'https://25664f771b26.ngrok.io';

    export default {
        name: "Login",
        data() {
            return {
                user: {
                    email: 'test@mail.com',
                    password: 'password'
                }
            }
        },
        methods: {
            onLogin() {
                let token = `Bearer ee724b02b67e5a0ef63f5f4b3c60d7bfa3cc90a157f644ad3f734a70212b172ec3f17375a3fcb968`;
                headers['Authorization'] = token;

                if (!this.echo) {
                    this.echo = new TnsEcho({
                        broadcaster: 'pusher',
                        host: apiUrl,
                        key: '158797eead9426132706',
                        authEndpoint: apiUrl + '/api/broadcasting/auth',
                        cluster: "ap2",
                        headers: {
                            auth: {
                                'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhdWQiOiI3IiwianRpIjoiOGM3YjEwZTA3ZDljZTVkN2JjNmUxYzEyNGRhMTljNjcwNjM3ZmIzMzRjNTQ4MjVjMmI3YzhkYzRhOTNmYTI5MWQ1NGMwYzVmNDgxZDlmMDAiLCJpYXQiOjE2MjAyMDQ5MjIsIm5iZiI6MTYyMDIwNDkyMiwiZXhwIjoxNjUxNzQwOTIyLCJzdWIiOiI0MSIsInNjb3BlcyI6W119.HHi-Q5BlRsE20WKM8Zznehm6utpMQYk_s2IqCjozBx_Lgq9UEtXUR2xZtbZbRa1xdq7-PD3qzneuxpEYx0sYRiHi_Dtxo7-rR_6yUULe45fRGQnmZ-uCbl5FqL3K9v1rhlY0JXHqJ1_kP3KbFK7dWVsGpfJXjkzmcU4BFX1wmCcGNL20LLS52OiGA0XcQggIGlyiGYRNrVjGNVnN7g3sF-r1vYldqtFsjvZLAERHP5jCR_xyEGTeA5BY-SAMNVKVmFR1NY4gxfHwoen-hK2tUQU6BQi_ywlAnhZWDLPEweFi1R5vAgTsUnpZL9Bp15nprxGZfKzvbjdLq0kVuweB_gmVEJeWCUF9YvoCBMixHJjzW893OFKjwqGxduDTTr8f9_JkcnuJwLmyE1_dWyWgMb95UgCIoojnxEpWNvhKMkL90Ge9bqrv2voCjeMwC1OmMggcwnl7rA27ksfGbMpNJzfltj27Tw0bI60a9IqSfHNSRJm6ZCcu34P8B9GlYJvYBsxuBS7dJCL_iJjBs-ufsuVVMDzXfFYYYtZBXltSs6n41bIJwRLwIYHkmGlzFXnTmq3HWuRXqldPF6ot8x9J0wCA4JHfuE2POS_v0Y5wopvS0dIXW_98827zo7RQQW_fnGgtGuq2qqmuCAcY_AoPymA1yhaC5SMmR1msj4ifzSU'
                            },
                        }
                    });
                }

                this.echo.connector.pusher.config.auth.headers = {
                    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhdWQiOiI3IiwianRpIjoiOGM3YjEwZTA3ZDljZTVkN2JjNmUxYzEyNGRhMTljNjcwNjM3ZmIzMzRjNTQ4MjVjMmI3YzhkYzRhOTNmYTI5MWQ1NGMwYzVmNDgxZDlmMDAiLCJpYXQiOjE2MjAyMDQ5MjIsIm5iZiI6MTYyMDIwNDkyMiwiZXhwIjoxNjUxNzQwOTIyLCJzdWIiOiI0MSIsInNjb3BlcyI6W119.HHi-Q5BlRsE20WKM8Zznehm6utpMQYk_s2IqCjozBx_Lgq9UEtXUR2xZtbZbRa1xdq7-PD3qzneuxpEYx0sYRiHi_Dtxo7-rR_6yUULe45fRGQnmZ-uCbl5FqL3K9v1rhlY0JXHqJ1_kP3KbFK7dWVsGpfJXjkzmcU4BFX1wmCcGNL20LLS52OiGA0XcQggIGlyiGYRNrVjGNVnN7g3sF-r1vYldqtFsjvZLAERHP5jCR_xyEGTeA5BY-SAMNVKVmFR1NY4gxfHwoen-hK2tUQU6BQi_ywlAnhZWDLPEweFi1R5vAgTsUnpZL9Bp15nprxGZfKzvbjdLq0kVuweB_gmVEJeWCUF9YvoCBMixHJjzW893OFKjwqGxduDTTr8f9_JkcnuJwLmyE1_dWyWgMb95UgCIoojnxEpWNvhKMkL90Ge9bqrv2voCjeMwC1OmMggcwnl7rA27ksfGbMpNJzfltj27Tw0bI60a9IqSfHNSRJm6ZCcu34P8B9GlYJvYBsxuBS7dJCL_iJjBs-ufsuVVMDzXfFYYYtZBXltSs6n41bIJwRLwIYHkmGlzFXnTmq3HWuRXqldPF6ot8x9J0wCA4JHfuE2POS_v0Y5wopvS0dIXW_98827zo7RQQW_fnGgtGuq2qqmuCAcY_AoPymA1yhaC5SMmR1msj4ifzSU'
                }
                console.log(this.echo.connector.pusher.config.auth.headers)
                // this.echo.connector.pusher.config.auth.headers['Authorization'] = 'Bearer eyJpdiI6IjVRUzA3VXlMRk81eEZxMDRDaUo4d1E9PSIsInZhbHVlIjoiSjJ3WnpkZis2b0RiOGJrVTgzNG1kUk1NM1Izak1EWUEwaUp0TEQvS1RUMHFOVWlUNlNnY3dLVW8wWjhIU285ZFY0cDZZYzNmK0Z6OTUvTkM3SVF2bU80MUJUU1RHOXRISWtpdjZ2aTdtN1RkZlFVZTJPL2Q4b2dtcCt2d3dWWk0iLCJtYWMiOiJlYmQyYTNmODk3Yzg4Mzk0YzM2MzhmNDgwOWIwZWRhMzk3NDg4YjY3Y2Y1MWNjYTc2MTVhY2UyZTBjOWE0YmFmIn0%3D';
                this.echo.private(`users.41`)
                    .notification((notification) => {
                        console.log(notification)
                    })


                // console.log(this.echo.connector.pusher.config.auth.headers)

                // setTimeout(() => {
                //     this.echo.connector.pusher.config.auth = {
                //         headers: {
                //             'Authorization':'Bearer eyJpdiI6IjVRUzA3VXlMRk81eEZxMDRDaUo4d1E9PSIsInZhbHVlIjoiSjJ3WnpkZis2b0RiOGJrVTgzNG1kUk1NM1Izak1EWUEwaUp0TEQvS1RUMHFOVWlUNlNnY3dLVW8wWjhIU285ZFY0cDZZYzNmK0Z6OTUvTkM3SVF2bU80MUJUU1RHOXRISWtpdjZ2aTdtN1RkZlFVZTJPL2Q4b2dtcCt2d3dWWk0iLCJtYWMiOiJlYmQyYTNmODk3Yzg4Mzk0YzM2MzhmNDgwOWIwZWRhMzk3NDg4YjY3Y2Y1MWNjYTc2MTVhY2UyZTBjOWE0YmFmIn0%3D'
                //         }
                //     }
                //     this.echo.private(`users.46`)
                //         .notification((notification) => {
                //             console.log(notification)
                //         })

                //     this.echo.private(`users.46`)
                //         .notification((notification) => {
                //             console.log(notification)
                //         })
                //     console.log(this.echo.socketId());
                //     console.log(this.echo.connector.pusher.config)
                //
                // }, 500);


                // console.log(this.echo.connector.pusher.config)
                console.log('q')


                this.echo.channel(`users`)
                    .listen('.OrderShipmentStatusUpdated', (e) => {
                        console.log(e)
                    })
                // http.request({
                //     url: `${apiUrl}/login`,
                //     method: 'POST',
                //     headers: {
                //         'Accept': 'application/json',
                //         'Content-type': 'application/json',
                //     },
                //     content: JSON.stringify(this.user)
                // }).then(({content}) => {
                //     console.log(content)
                //     let data = content.toJSON();
                //     this.$localStorage.setItem('token', data.token);
                //     this.$navigateTo(App)
                // })
            }
        }
    };
</script>

<style scoped>

</style>
