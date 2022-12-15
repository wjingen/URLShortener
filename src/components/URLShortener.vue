<template>
	<div class="main">
		<header class="header"> 
			<h1> Welcome to Shorten My URL!</h1>
		</header>
		<body class="body">
			<text class="website-name">Shorten My URL</text>
			<text> Tinyurl shortener is a free online tool that shortens any url for free. Established by a group of Harvard students in 2012, tinyurl shortener shortens over 19,000 urls daily. Say goodbye to long, chunky URLs!</text>
			<v-text-field label="Type in your URL here" type="text" v-model="urlValue"/>
			<v-btn @click="submitUrl(urlValue)" color="black" :loading="loading">Shorten!</v-btn>
		</body>
		<body class="body" v-if="(submitted && !error)" >
			<h2 style="position:relative;left:-40px">Your shortened URL is below:</h2>
			<v-text-field label="Your shortened URL" type="text" v-model="newUrl"/>
			<div class="button">
				<v-btn color="red" width="45%" @click="copy()">
					Copy Link
					<v-icon right dark>
						mdi-content-copy
					</v-icon>
				</v-btn>
				<v-btn color="green" width="45%" @click="reset()">
					Reset
					<v-icon right dark>
						mdi-export-variant
					</v-icon>
				</v-btn>
			</div>
			<div class="alert">
				<v-alert v-model="alert" color="pink">
					Your link has been copied.
				</v-alert>
			</div>
		</body>
		<body class="body" v-if="error">
			<h3>Error</h3>
			Sorry, we couldn't shorten the url. Please check if the URL provided is valid, if not, refresh the website and try again.
			<v-btn @click="refreshPage()" color="green"> Refresh Page</v-btn>
		</body>
		<body class="flex-box">
			<URLComponentVue title="Easy" icon="mdi-thumb-up-outline" message="Shorten My URL is easy and fast, enter the long link to get your shortened link"/>
			<URLComponentVue title="Shortened" icon="mdi-content-copy"  message="Use any link, no matter what size, Shorten My URL always shortens"/>
			<URLComponentVue title="Secure" icon="mdi-safe" message="It is fast and secure, our service have HTTPS protocol and data encryption"/>
			<URLComponentVue title="Statistics" icon="mdi-database" message="Check the amount of clicks that your shortened url received"/>
			<URLComponentVue title="Reliable" icon="mdi-hand" message="All links that try to disseminate spam, viruses and malware are deleted"/>
			<URLComponentVue title="Devices" icon="mdi-devices" message="Compatible with smartphones, tablets and desktop"/>
		</body>
		<div class="footer">
			<text> © 2022 ShortenMyUrl.at - Tool to shorten a long link. Created by <b>Jing En</b> </text>
			<div class="credits">
				<a class="link" href="https://www.linkedin.com/in/jingenwong/">LinkedIn Profile</a>
				<a class="link" href="https://github.com/wjingen/">GitHub Repository</a>
			</div>
			
		</div>
	</div>
</template>

<script>
import URLComponentVue from './URLComponent.vue';
	export default {
		data() {
			return {
				urlValue: "",
				processedUrl: "",
				data: null,
				newUrl: "",
				submitted: false,
				error: false,
				alert: false,
				loading: false
			}
		},
		components: { 
			URLComponentVue 
		},
		methods: {
			submitUrl(urlValue) {
				this.loading = true
				this.processedUrl = this.processUrl(urlValue)
				this.getShortenedUrl()
			},
			processUrl(urlValue) {
				if (urlValue.startsWith("https://") || urlValue.startsWith("http://")) {
					return urlValue;
				} else {
					return "https://" + urlValue;
				}
			},
			getShortenedUrl() {
				this.submitted = true;
				console.log(import.meta.env.VITE_BEARER_TOKEN)
				fetch('https://api-ssl.bitly.com/v4/shorten', {
					method: 'POST',
					headers: {
						'Authorization': 'Bearer ' + import.meta.env.VITE_BEARER_TOKEN,
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({ "long_url": this.processedUrl, "domain": "bit.ly", "group_guid": "" })
				}).then(response => response.json()).then(data => {
					if (data.message === "INVALID_ARG_LONG_URL" || data.message === "ALREADY_A_BITLY_LINK") {this.error = true; return;}
					this.error = false
					this.data = data
					this.newUrl = data.link}
				)
				this.loading = false
				console.log("loading is",loading)
			},
			copy() {
				this.alert = true;
				navigator.clipboard.writeText(this.newUrl);
				window.setInterval(() => {
					this.alert = false;
				}, 3000)
			},
			reset() {
				this.urlValue = "",
				this.processedUrl = "",
				this.data = null,
				this.newUrl = "",
				this.submitted = false,
				this.error = false,
				this.alert = false,
				this.loading = false
			},
			refreshPage() {
				window.location = window.location.href;
			}
		}
	}
</script>

<style lang="scss" scoped>
.main {
	position: relative;
	background-color: rgb(250,250,250);
	width: "100%";
	height: "100%";
	display: flex;
	flex-flow: column nowrap;
	justify-content: baseline;
}
.header {
	border-bottom: 2px solid black;
}
.header h1 {
	text-align: center;
	font-family:'Courier New', Courier, monospace;
}
.body {
	padding: 50px 50px;
	margin: 10px;
	text-align: center;
	width: 600px;
	margin-left: auto;
	margin-right: auto;
	display: flex;
	flex-direction: column;
	align-items: left;
	background-color: white;
	box-shadow: 2px 2px grey;
}

.body .website-name {
	align-self: center;
	font-weight: 700;
	font-size: 30px;
	font-family: Impact, Haettenschweiler, 'Arial', sans-serif;
	color: black
}
.body label {
	font-weight: bold;
}
.error {
	padding: 30px 0;
	text-align: center;
	max-width: 400px;
	display: flex;
	margin-left: auto;
	margin-right: auto;
	flex-direction: column;
}
.body .button {
	display: flex;
	flex-flow: row wrap;
	justify-content: space-evenly;
}
.flex-box {
	display: flex;
	flex-direction: row;
	flex-wrap: wrap;
	justify-content: space-evenly;
	text-align: center;
	max-width: 650px;
	margin-left: auto;
	margin-right: auto;
}
.footer {
	text-align: center;
	height: 110px;
	width: 100%;
	background-color: #333333;
	border-top: 6px solid #00a0db;
	color: white;
	display: flex;
	flex-direction: column;
	align-content: center;
	justify-content: center;
}

.footer .credits {
	display: flex;
	flex-direction: row;
	justify-content: space-evenly;

}
.footer .link {
	color: #1a5c96;
	text-decoration: none;
}
.footer .link:hover {
	color: grey;
	text-decoration: underline;
}
.footer .link:visited {
	color: lightblue;
}

</style>