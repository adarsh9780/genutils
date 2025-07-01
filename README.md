```javascript

// hooks/useBackendStatus.js
import { useQuery } from "@tanstack/react-query";

export default function useBackendStatus() {
  return useQuery({
    queryKey: ["health"],
    queryFn: async () => {
      const res = await fetch("/health");
      if (!res.ok) throw new Error("Backend is unhealthy");
      return res.json();
    },
    refetchInterval: 10000, // Check every 10 seconds
    retry: false, // No retry if it fails
    staleTime: 5000, // Prevent flickering
  });
}


// components/BackendStatusBanner.js
import useBackendStatus from "../hooks/useBackendStatus";

export default function BackendStatusBanner() {
  const { isError, isLoading } = useBackendStatus();

  if (isLoading) return null; // show nothing while checking
  if (isError) {
    return (
      <div style={{ backgroundColor: "#ffdddd", padding: "10px", textAlign: "center", color: "#a00" }}>
        ⚠️ Backend is currently unavailable. Please try again later.
      </div>
    );
  }

  return null;
}


// main.jsx or index.js
import React from "react";
import ReactDOM from "react-dom/client";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import App from "./App";

const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </React.StrictMode>
);

// App.jsx
import BackendStatusBanner from "./components/BackendStatusBanner";

function App() {
  return (
    <div>
      <BackendStatusBanner />
      <h1>🚀 My React App</h1>
      {/* Other UI */}
    </div>
  );
}

export default App;



```
