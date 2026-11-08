
# First run (build, create, and start the bot and database containers):
docker compose -f docker-compose.web.yml up -d

# Open http://localhost:8080/ and enter the Steam Guard code when prompted.

# Stop the bot and database containers:
docker compose -f docker-compose.web.yml stop

# Start existing containers again after stopping them:
docker compose -f docker-compose.web.yml start

# Restart the bot only:
docker compose -f docker-compose.web.yml restart node-chrome

# Check the status:
docker compose -f docker-compose.web.yml ps

# Command	                                                    What it does
@ghmusic youtube lo-fi hip hop	                                Search YouTube and play the first result
@ghmusic youtube dramatic chipmunk #2	                        Play a specific search result
@ghmusic playurl https://youtube.com/watch?v=VIDEO_ID	        Play audio from a URL, including YouTube links
@ghmusic play songname	                                        Play an uploaded sound matching the name
@ghmusic stop	                                                Stop the current playback
@ghmusic pause	                                                Stop the current playback
@ghmusic play	                                                Resume stopped/paused playback, without an argument