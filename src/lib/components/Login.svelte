<script>
  import { createEventDispatcher } from "svelte";

  export let apiEndpoint = "/api/login"; // Default endpoint, can be overridden by prop

  const dispatch = createEventDispatcher();

  let username = "";
  let password = "";
  let confirmPass = "";
  let isLoading = false;
  let error = "";

  async function handleLogin() {
    if (!username || !password) {
      error = "Please fill in all fields";
      dispatch("loginError", { error });
      return;
    }

    isLoading = true;
    error = "";

    try {
      const response = await fetch(apiEndpoint, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ username, password }),
      });

      const data = await response.json();

      if (!response.ok) {
        throw new Error(data.message || "Login failed");
      }

      dispatch("loginSuccess", { data });
    } catch (err) {
      error = err.message;
      dispatch("loginError", { error });
    } finally {
      isLoading = false;
    }
  }

  function handleForgotPassword() {
    dispatch("forgotPassword");
  }
</script>

<div class="max-w-md mx-auto p-8 bg-gray-500 rounded-lg shadow-md">
  <form on:submit|preventDefault={handleLogin} class="space-y-6">
    <div class="space-y-2">
      <label for="username" class="block text-sm font-medium text-gray-700"
        >Username</label
      >
      <input
        type="text"
        id="username"
        bind:value={username}
        placeholder="Enter your username"
        disabled={isLoading}
        class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 disabled:bg-gray-100 disabled:cursor-not-allowed"
      />
    </div>

    <div class="space-y-2">
      <label for="password" class="block text-sm font-medium text-gray-700"
        >Password</label
      >
      <input
        type="password"
        id="password"
        bind:value={password}
        placeholder="Enter your password"
        disabled={isLoading}
        class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 disabled:bg-gray-100 disabled:cursor-not-allowed"
      />
    </div>
    <div class="space-y-2">
      <label for="password" class="block text-sm font-medium text-gray-700"
        >Confirm Password</label
      >
      <input
        type="password"
        id="confirmPass"
        bind:value={confirmPass}
        placeholder="Re-enter your password"
        disabled={isLoading}
        class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 disabled:bg-gray-100 disabled:cursor-not-allowed"
      />
    </div>

    {#if error}
      <div class="text-sm text-red-600">{error}</div>
    {/if}

    <button
      type="submit"
      disabled={isLoading}
      class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 disabled:bg-gray-400 disabled:cursor-not-allowed transition-colors duration-200"
    >
      {isLoading ? "Logging in..." : "Login"}
    </button>

    <button
      type="button"
      on:click={handleForgotPassword}
      class="w-full text-sm text-blue-600 hover:text-blue-800 underline focus:outline-none transition-colors duration-200"
    >
      Forgot Password?
    </button>
  </form>
</div>
