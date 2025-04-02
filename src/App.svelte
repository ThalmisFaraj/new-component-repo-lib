<script>
  import svelteLogo from "./assets/svelte.svg";
  import viteLogo from "/vite.svg";
  import Login from "./lib/components/Login.svelte";

  function handleLoginSuccess(event) {
    console.log("Login successful:", event.detail.data);
    // Handle successful login here
  }

  function handleLoginError(event) {
    console.error("Login error:", event.detail.error);
    // Handle login error here
  }

  function handleForgotPassword() {
    console.log("Forgot password clicked");
    // Handle forgot password here
  }
</script>

<main class="min-h-screen bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
  <h1>New Component Library</h1>
  <div class="mt-8">
    <Login
      apiEndpoint="/api/login"
      on:loginSuccess={handleLoginSuccess}
      on:loginError={handleLoginError}
      on:forgotPassword={handleForgotPassword}
    />
  </div>
</main>
