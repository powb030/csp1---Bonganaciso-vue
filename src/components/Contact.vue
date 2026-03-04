<template>
<section id="contact" class="bg-dark py-5">
	    <div class="container-fluid">
	        <div class="text-center">
	            <h3 id="text-contact" class="mb-3 text-white">Contacts</h3>
	        </div>

	        <div class="row g-4" id="map">

	            <!-- Map Column -->
	            <div class="col-12 col-md-6 d-flex flex-column align-items-center">
	                
	                <h2 class="mb-3 text-white text-center">Location</h2>

	                <iframe 
	                    src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d247029.68727852078!2d120.26807557949314!3d14.665084304246395!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x339614e301987889%3A0x6ae0cb8f5ee13d48!2sBataan!5e0!3m2!1sen!2sph!4v1764325348024!5m2!1sen!2sph"
	                    width="75%" height="400" style="border:0;"
	                    allowfullscreen loading="lazy">
	                </iframe>

	            </div>

	            <!-- Contact Form -->
	            <div class="col-12 col-md-6 d-flex justify-content-center">
	                <div class="contact-form-wrapper w-100 px-md-5 px-3 border border-2 border-dark rounded bg-secondary">
	                    <form class="mt-4">
	                        <div class="mb-3">
	                            <label for="name" class="form-label">Name:</label>
	                            <input type="text" class="form-control custom-input" id="name" placeholder="Name">
	                        </div>

	                        <div class="mb-3">
	                            <label for="email" class="form-label">Email Address:</label>
	                            <input type="email" class="form-control custom-input" id="email" placeholder="Email">
	                        </div>

	                        <div class="mb-3">
	                            <label for="message" class="form-label">Message:</label>
	                            <textarea class="form-control custom-input" id="message" rows="5"></textarea>
	                        </div>

	                        <div class="d-flex justify-content-center pb-3">
	                            <button type="submit" class="btn custom-btn border-dark form-label">Submit</button>
	                        </div>
	                    </form>
	                </div>
	            </div>


	            <div class="d-flex text-center justify-content-center">
	            	<div class="col-12 col-md-6" >
	            		
	            		 <div class="col-12 mx-auto mt-3">
	            		    <a href="https://www.facebook.com/login/" target="_blank"><img src="/images/fb.png" alt="Facebook" class="logo" width="50"></a>
	            		    <a href="https://www.linkedin.com/login" target="_blank"><img src="/images/linkedin.png" alt="Linkedin" class="logo" width="38"></a>
	            		    <a href="https://github.com/login" target="_blank"><img src="/images/gitwhite.png" alt="GitHub" class="logo" width="40"></a>
	            		 </div>

	            	</div>
	            </div>
	        </div>
	    </div>
	</section>

</template>

<script setup>

	import {ref, onMounted, onBeforeUnmount} from "vue";

	import {Notyf} from "notyf";
	import "notyf/notyf.min.css";

	const notyf = new Notyf();

	const WEB3FORMS_ACCESS_KEY = "24c8a47f-f440-4a16-b5c8-e2e3679a331b";

	// email subject
	const subject = "New message from Portfolio Contact form";

	const name = ref("");
	const email = ref("");
	const message = ref("");

	const isLoading = ref(false);

	const submitForm = async()=> {

		// Check if reCAPTCHA token is present, return an error when not verified
		if(!recaptchaToken.value){
			notyf.error("Please verify that you are not a robot")
		}

		isLoading.value = true;


		try{
			const response = await fetch("https://api.web3forms.com/submit", {
				method: "POST",
				headers: {
					"Content-Type" : "application/json",
					Accept: "application/json"
				},
				body: JSON.stringify({
					access_key: WEB3FORMS_ACCESS_KEY,
					subject: subject,
					name: name.value,
					email: email.value,
					message: message.value
				})
			})

			const result = await response.json();

			if(result.success){
				console.log(result);
				isLoading.value = false;
				notyf.success("Message Sent!");
			}
			else{
				isLoading.value = false;
				notyf.error("Failed to send message");
			}
		}
		catch(error){
			console.log(error);
			isLoading.value = false;
			notyf.error("Failed to send message");
		}
		

	}
	


	const SITE_KEY = '6Ld2XH8sAAAAAN9k_kuafWfxXS44QgJxVogIRCan';  

	const recaptchaContainer = ref(null);
	const recaptchaWidgetId = ref(null);
	const recaptchaToken = ref('');

	// Callback called by reCAPTCHA when successful
	function onRecaptchaSuccess(token) {
	  recaptchaToken.value = token;
	}

	// Callback when expired
	function onRecaptchaExpired() {
	  recaptchaToken.value = '';
	}

	// Function to render the reCAPTCHA widget
	function renderRecaptcha() {
	  if (!window.grecaptcha) {
	    console.error('reCAPTCHA not loaded');
	    return;
	  }

	  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
	    sitekey: SITE_KEY,
	    size: 'normal', // or 'compact'
	    callback: onRecaptchaSuccess,
	    'expired-callback': onRecaptchaExpired,
	  });
	}

	// Function to reset reCAPTCHA 
	function resetRecaptcha() {
	  if (recaptchaWidgetId.value !== null) {
	    window.grecaptcha.reset(recaptchaWidgetId.value);
	    recaptchaToken.value = '';
	  }
	}



	onMounted(() => {
	  // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
	  // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
	  // Callback functions handle success and expiration events. 
	  // reCAPTCHA is reset upon form submission to clear the token.
	  const interval = setInterval(() => {
	    if (window.grecaptcha && window.grecaptcha.render) {
	      renderRecaptcha();
	      clearInterval(interval);
	    }
	  }, 100);

	  onBeforeUnmount(() => {
	    clearInterval(interval);
	  });
	});

	



</script>