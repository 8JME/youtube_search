<template>
    <div class="container">
        <SearchBar @searchChange="onSearchChange"></SearchBar>
        <div class="row">
            <VideoDetail :video="selectedVideo" />
            <VideoList @videoSelect="onVideoSelect" :videos="videos"></VideoList>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import SearchBar from './components/SearchBar';
    import VideoList from './components/VideoList';
    import VideoDetail from './components/VideoDetail';

    const API_KEY = 'Your Google API goes here';
    export default {
        name: 'App',
        components: {
            SearchBar,
            VideoList,
            VideoDetail
        },
        data() {
            return { videos: [], selectedVideo:  null };
        },
        methods: {
            onVideoSelect(video) {
                this.selectedVideo = video;
            },
            onSearchChange(searchChange) {
                axios
                    .get('https://www.googleapis.com/youtube/v3/search', {
                    params: {
                        key: API_KEY,
                        type: 'video',
                        part: 'snippet',
                        q: searchChange
                    }
                }).then(response => {
                    this.videos = response.data.items;
                });
            }
        }
    };
</script>