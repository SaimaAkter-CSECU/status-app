<template>
    <v-container class="mt-5">
        <v-card
            class="mx-auto rounded-lg px-3 mb-5"
            light
            max-width="700"
        >
            <v-card-text>
                <v-list-item class="grow">
                    <v-list-item-avatar color="grey darken-3">
                        <v-img
                            class="elevation-6"
                            alt=""
                            src= "https://avataaars.io/"
                        >
                        </v-img>
                    </v-list-item-avatar>

                    <v-list-item-content>
                        <v-dialog
                            v-model="dialog"
                            width="700"
                        >
                            <template v-slot:activator="{ on, attrs }">
                                <v-btn
                                    block
                                    rounded
                                    elevation="2"
                                    plain
                                    x-large
                                    color= "blue-grey"
                                    class="status-btn"
                                    v-bind="attrs"
                                    v-on="on"
                                >
                                    What's on your mind, Saima?
                                </v-btn>    
                            </template>

                            <v-card>
                                <v-card-title class="py-4">
                                    <span class="text-h5 mx-auto font-weight-bold">Create Post</span>
                                    <v-icon
                                        large
                                        right
                                        @click="dialog = false"
                                    >
                                        mdi-close-circle-outline
                                    </v-icon>
                                </v-card-title>

                                <v-divider></v-divider>

                                <v-card-text>
                                    <v-row>
                                        <v-col cols="12">
                                            <v-list-item class="grow">
                                                <v-list-item-avatar color="grey darken-3">
                                                    <v-img
                                                        class="elevation-6"
                                                        alt=""
                                                        src= "https://avataaars.io/"
                                                    >
                                                    </v-img>
                                                </v-list-item-avatar>
                                                <v-list-item-content color="grey darken-3">
                                                    <div class="d-flex flex-column">
                                                        <h5 class="font-weight-bold">Saima</h5>
                                                        <v-select
                                                            :items="items"
                                                            v-model="defaultSelected"
                                                            solo
                                                        ></v-select>
                                                    </div>
                                                </v-list-item-content>
                                            </v-list-item>
                                        </v-col>
                                        <v-col cols="12">
                                            <v-form
                                            ref="form"
                                            >
                                                <v-row>
                                                    <v-col cols="12">
                                                        <v-textarea
                                                        color="teal"
                                                        v-model= "statusDescription"
                                                        placeholder="What's on your mind, Saima?"
                                                        required
                                                        >
                                                        </v-textarea>
                                                    </v-col>
                                                </v-row>
                                                
                                                <v-divider></v-divider>

                                                <v-card-actions class="pt-3">
                                                    <v-btn
                                                        color="primary"
                                                        block
                                                        :disabled="!statusDescription" 
                                                        @click="addStatus(); dialog = false; snackbar = true"
                                                    >
                                                        Post
                                                    </v-btn>
                                                </v-card-actions>
                                            </v-form>
                                        </v-col>
                                    </v-row>
                                </v-card-text>
                            </v-card>
                        </v-dialog>   
                    </v-list-item-content>
                </v-list-item>
            </v-card-text>

            <v-snackbar
                v-model="snackbar"
                :timeout="timeout"
                right
                shaped
                top
                color="blue-grey"
            >
                Your Status is Posted

                <template v-slot:action="{ attrs }">
                    <v-btn
                    color="blue"
                    text
                    v-bind="attrs"
                    @click="snackbar = false"
                    >
                        <v-icon
                            right
                            @click="dialog = false"
                        >
                            mdi-close-circle-outline
                        </v-icon>
                    </v-btn>
                </template>
            </v-snackbar>

            <v-divider></v-divider>

            <v-card-actions class="card-footer pa-4">
                <v-row
                align="center"
                justify="space-around"
                >
                    <v-btn
                        class="ma-2"
                        plain
                    >
                        <v-icon
                        large
                        color="red"
                        left
                        class="pe-4"
                        >
                            mdi-video
                        </v-icon>
                        Live Video
                    </v-btn>
                    <v-btn
                        class="ma-2"
                        plain
                    >
                        <v-icon
                        color="green"
                        large
                        left
                        class="pe-4"
                        >
                        mdi-image-multiple
                        </v-icon>
                        Photo/Video
                    </v-btn>
                    <v-btn
                        class="ma-2"
                        plain
                    >
                        <v-icon
                        color="yellow"
                        large
                        left
                        class="pe-4"
                        >
                        mdi-emoticon
                        </v-icon>
                        Feeling/activity
                    </v-btn>


                </v-row>

            </v-card-actions>
        </v-card>

        <v-container>
            <v-card
                class="mx-auto rounded-lg px-3 mb-5"
                light
                max-width="700"
                v-for="(stat, index) in statuses" 
                :key="index"
            >
                <v-card-text>
                    <v-row>
                        <v-col cols="12" class="pb-0">
                            <v-list-item class="grow d-flex align-items-center my-0">
                                <v-list-item-avatar color="grey darken-3 p-0">
                                    <v-img
                                        class="elevation-6"
                                        alt="Profile Picture"
                                        :src= "stat.src"
                                    >
                                    </v-img>
                                </v-list-item-avatar>
                                <v-list-item-content color="grey darken-3">
                                    <div class="d-flex flex-column">
                                        <h5 class="font-weight-bold mb-1">{{stat.name}}</h5>
                                        <p class=" mb-0">
                                            {{stat.time}} . 
                                            <span v-if="stat.privacy === 'Public'">
                                                <v-icon
                                                color="gray"
                                                left
                                                small
                                                >
                                                    mdi-earth
                                                </v-icon>
                                            </span>
                                            <span v-else-if="stat.privacy === 'Friends'">
                                                <v-icon
                                                color="gray"
                                                left
                                                small
                                                >
                                                    mdi-account-multiple
                                                </v-icon>
                                            </span>
                                            <span v-else>
                                                <v-icon
                                                color="gray"
                                                left
                                                small
                                                >
                                                    mdi-lock
                                                </v-icon>
                                            </span>
                                        </p>
                                    </div>
                                </v-list-item-content>
                            </v-list-item>
                        </v-col>
                        <v-col cols="12">
                            <h3 class="font-weight-medium">{{stat.status}}</h3>
                        </v-col>
                        <v-col cols="12">
                            <v-divider></v-divider>
                            <v-row
                                align="center"
                                justify="space-around"
                                class="my-2"
                            >
                                <div>
                                    <v-btn
                                        text
                                        v-if="stat.like"
                                        color="primary"
                                        @click="toggleLike(index)"
                                    >
                                        <v-icon left>
                                            mdi-thumb-up
                                        </v-icon>
                                        Liked
                                    </v-btn>
                                    <v-btn
                                        text
                                        v-else
                                        @click="toggleLike(index)"
                                    >
                                        <v-icon left>
                                            mdi-thumb-up-outline
                                        </v-icon>
                                        Like
                                    </v-btn>

                                </div>
                                <v-btn
                                    text
                                >
                                    <v-icon left>
                                        mdi-comment-outline
                                    </v-icon>
                                    Comment
                                </v-btn>                                
                            </v-row>
                            <v-divider></v-divider>
                            <v-row
                                align="center"
                                justify="start"
                                class="my-1"
                            >
                                <v-col cols="12" class="pb-0">
                                    <v-list-item class="grow d-flex align-items-center my-0">
                                        <v-list-item-avatar color="grey darken-3 p-0" class="my-0">
                                            <v-img
                                                class="elevation-6"
                                                alt="Profile Picture"
                                                :src= "stat.src"
                                            >
                                            </v-img>
                                        </v-list-item-avatar>
                                        <v-list-item-content color="grey darken-3" class="py-0">
                                            <v-text-field
                                                placeholder="Write a comment...."
                                                append-icon="mdi-emoticon-happy-outline"
                                                solo
                                                rounded
                                                
                                            ></v-text-field>
                                        </v-list-item-content>
                                    </v-list-item>
                                </v-col>                              
                            </v-row>
                        </v-col>
                    </v-row>
                </v-card-text>
            </v-card>
        </v-container>
    </v-container>
</template>

<script>
    export default {
        name: 'StatusBox',
        data: () => ({
            dialog: false,
            snackbar: false,
            timeout: 2000,
            items: ['Public', 'Friends', 'Only Me'],
            defaultSelected: 'Public', 
            statusDescription: '',
            statuses : [
                {
                    name: 'User1',
                    status: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industrys standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.',
                    src: 'https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light',
                    time: '3h',
                    privacy: 'Public',
                    like: false
                },
                {
                    name: 'User2',
                    status: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industrys standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.',
                    src: 'https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light',
                    time: '4h',
                    privacy: 'Friends',
                    like: false
                },
                {
                    name: 'User3',
                    status: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industrys standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.',
                    src: 'https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light',
                    time: '19h',
                    privacy: 'Only Me',
                    like: false
                },
            ]
        }),

        methods: {
            addStatus(){
                if(this.statusDescription.length === 0 ) return;
                this.statuses.unshift({
                    name: 'Saima Akter', 
                    status: this.statusDescription,
                    src: 'https://avataaars.io/',
                    time: 'Just now',
                    privacy: this.defaultSelected, 
                    like: false
                });
                this.statusDescription = '';
                this.defaultSelected= 'Public';
            },
            toggleLike(index){
                this.statuses[index].like = !(this.statuses[index].like); 
            }
        },
    }
</script>