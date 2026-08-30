# Streaming Blog-Post Workflow

This is the standard workflow after every completed stream.

These posts are primarily a simple chronological record, not long reviews or
technical essays. Keep each post concise: identify what was streamed, record
the approximate duration and outcome, include Ashwin's optional comment, link
the published video and playlist, and note that the recording was saved. Add
more only when something genuinely notable happened.

## 1. Ask Ashwin for a comment

Before writing the final post, briefly ask Ashwin whether he has anything he
wants to say about the stream. This may concern the game or activity, notable
moments, technical problems, the streaming setup, or anything else he wants in
the post.

Do not publish the post until his answer has either been incorporated or he has
said that he has nothing to add.

### Writing before the archive and upload finish

Ashwin may ask for the blog post to be written first while the recording is
still local. That is fine: create the post, its cover, and the factual stream
summary after validating the local recording. Do not add a made-up YouTube
embed, claim that the archive is complete, or publish the post as though those
later steps had succeeded. Return to the same post after the checksum-verified
NFS export and YouTube upload, then add the real video shortcode and playlist
link before publication.

## 2. Validate the local recording

Inspect the completed recording in `~/Streams` and record at least:

- start date and time;
- duration;
- file size;
- video resolution and frame rate;
- video and audio codecs.

Confirm that the recording can be read before archiving or uploading it.

## 3. Archive the recording on the fileserver

The Slacktop-facing archive directory is:

```text
/var/nfs/streams/exported
```

Create `exported` if it does not yet exist.

Naming is stream-type-specific. Before choosing a destination name, inspect the
existing files in `/var/nfs/streams` and the relevant category subdirectory.
Preserve the established vocabulary and numbering for that type.

Examples already in use include:

```text
gaming/YYYY-MM-DD-gaming-mkw[-NN].mkv
irl/YYYY-MM-DD-irl-<activity>[-NN].mkv
sport/YYYY-MM-DD-sport-<activity>[-NN][-stabilized].mkv
language/YYYY-MM-DD-language-<language-or-format>.mkv
math/YYYY-MM-DD-<math-description>.mkv
vietnamese lessons/NNN-YYYY-MM-DD.mkv
```

For newly exported recordings, include the recording start time down to the
minute when Ashwin requests it. The current Mario Kart World export uses:

```text
YYYY-MM-DD-HH-MM-mkw.mkv
```

2026-08-29-08-57-mkw.mkv

`mkw` means `Mario Kart World`. Do not automatically apply the M.K.W. pattern
to language, IRL, sport, mathematics, lesson, or other recordings. Derive those
names from their own existing category convention and ask Ashwin if the match
is ambiguous.

Preserve the original local recording. After the transfer, compare a SHA-256
checksum of the fileserver copy with the local source. The archive is complete
only when the checksums match.

## 4. Upload the high-quality master to YouTube

Use the local recording as the YouTube upload source unless Ashwin explicitly
chooses the lower-quality livestream replay. The local recording is normally
the better master because it avoids livestream bitrate limits, relay-path
losses, and YouTube's live transcoding.

Use a clear title and description based on the stream. Do not expose stream
keys, relay credentials, private addresses, or operational secrets.

## 5. Add the video to the appropriate playlist

Create a public game or activity playlist when one does not already exist. Use
the canonical subject name; for example:

```text
Mario Kart World
```

Add the newly uploaded video to that playlist. Avoid duplicate playlists and
duplicate playlist entries.

## 6. Create a fresh cover image

Every stream post must contain its own image at:

```text
content/post/<post-slug>/cover.png
```

Generate a new 16:9 cover appropriate to the stream. Keep visual variety between
posts while making the subject recognizable. Do not reuse a generic cover by
default. Avoid watermarks, accidental text, copied official promotional art,
and exposed private information.

The post front matter must reference:

```yaml
image: "cover.png"
```

## 7. Write the post

Create the post at:

```text
content/post/<post-slug>/index.md
```

Use the existing Hugo front-matter conventions. Include suitable categories,
tags, a description, `draft: false`, and `comments: true`.

Every post written through this workflow must introduce its writer explicitly:

```markdown
Hello. I am **Anonymous Idiot**, today's guest writer.
```

The exact surrounding wording may vary, but the guest-writer name must remain
**Anonymous Idiot**. The post should also end with a brief Anonymous Idiot
byline.

Include:

- what was streamed;
- the approximate duration;
- anything Ashwin asked to include;
- notable technical or gameplay events when relevant;
- the YouTube video embedded with Hugo's built-in `youtube` shortcode once
  available;
- a link to the appropriate YouTube playlist.

Use the YouTube video ID, not the complete URL:

```text
{{< youtube VIDEO_ID >}}
```

Keep the playlist as an ordinary Markdown link below or near the player. A
plain video link is only a fallback for a video that cannot be embedded.

Do not invent Ashwin's opinions, reactions, or gameplay details.

Keep relay architecture and other infrastructure details out of ordinary
stream posts. Include a technical fact only when an ordinary reader can
understand it immediately and it directly matters to the stream—for example,
that a local recording was saved and archived. Save explanations of VPS relay
routing, distribution architecture, platform plumbing, OBS internals, and
similar implementation details for a separate technical post unless Ashwin
explicitly asks to include them.

## 8. Validate and publish

Before publication:

1. Confirm the website worktree contains only the intended post and related
   files, apart from unrelated pre-existing user changes.
2. Build the Hugo site locally and resolve build errors.
3. Check that the post appears in the generated site and that `cover.png` is
   processed correctly.
4. Review the final diff.
5. Commit only the intended paths with a descriptive commit message.
6. Push and deploy using the site's canonical update workflow.
7. Verify that the public post renders the playable YouTube embed and that the
   video and playlist work.

Do not claim completion while a large upload, YouTube processing, checksum,
website deployment, or public verification is still pending.

## Deferred PeerTube migration

YouTube remains the current video-publication platform. The long-term hobby
plan is to migrate the stream/video catalogue and the corresponding blog-post
links to PeerTube after both of these prerequisites are satisfied:

1. a PeerTube instance has been set up and validated;
2. Ashwin's upload bandwidth has become reliably better.

The checksummed fileserver recordings are the canonical migration sources. Do
not begin a bulk PeerTube upload or rewrite historical post links before both
prerequisites are confirmed.
