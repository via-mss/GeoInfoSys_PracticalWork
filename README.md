Leaflet Web Map — WMS + Vector

This small web map demonstrates:
- One WMS layer (tile WMS service)
- Administrative boundaries loaded from `lv.json`
- Interactive vector popups, hover highlight, selection, and user-draw tools

Files of interest
- `index.html` — main application
- `lv.json` — administrative boundaries (GeoJSON)

Important: For browsers to fetch local files (lv.json / GeoJSON), you must serve the folder over HTTP. Loading `index.html` via `file://` will result in fetch() errors.

Quick ways to serve the folder (Windows PowerShell):

# Using Python 3 (if installed)
# Python 3.7+ (run from project folder)
python -m http.server 8000

# Using Node (http-server)
# Install once: npm install -g http-server
http-server -p 8000

# Using Live Server (VS Code extension)
# Right-click index.html -> "Open with Live Server"

Then open http://localhost:8000 in your browser.

Notes
- The page includes external CDN links for Leaflet and Leaflet.draw.
-- If you want to change the WMS source, update the WMS1 constant in `index.html`.

Contact
If you want additional features (search, feature filtering, printing, or export), open an issue or ask for enhancements.

Server-side hosting tips (Nginx + CORS)

If you host this on a server (recommended for production), here's a minimal Nginx configuration snippet to serve the static files and enable CORS for GeoJSON requests:

server {
	listen 80;
	server_name your.domain.example;
	root /var/www/gys; # point to the folder with index.html

	location / {
		try_files $uri $uri/ =404;
	}

	# Allow CORS for GeoJSON (if requested from other origins)
	location ~* \.(json|geojson)$ {
		add_header Access-Control-Allow-Origin "*";
		add_header Access-Control-Allow-Methods "GET, OPTIONS";
		add_header Access-Control-Allow-Headers "DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range";
		add_header Access-Control-Expose-Headers "Content-Length,Content-Range";
	}
}

Notes on CORS: If your GeoJSON files are hosted on the same domain as the website, you don't need special CORS headers. If they're hosted elsewhere, ensure the server serving the GeoJSON allows cross-origin GET requests (Access-Control-Allow-Origin). For restricted production setups, replace "*" with the specific allowed origin.

TLS: In production, enable TLS (Let's Encrypt) and redirect HTTP to HTTPS.