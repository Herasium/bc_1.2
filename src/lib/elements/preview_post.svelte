<script>
    // @ts-nocheck
    
        import bc from "$lib/img/logo_blank.svg"
        import {badges,titles} from "$lib/badges.js"
    import SvelteMarkdown from 'svelte-markdown'
    
        export let post
        
        function formatDate(unixTimestamp) {
            const date = new Date(unixTimestamp * 1000);
            const options = { 
                year: 'numeric', 
                month: 'long', 
                day: 'numeric', 
                hour: '2-digit', 
                minute: '2-digit'
            };
            return date.toLocaleDateString('en-EN', options);
        }
        const source = post["data"]["data"]
    </script>
    
    
    <div id="post" class="post">
        <div id="top-container">
            <a href="/profile/{post["buyer"] ? post["buyer"]["user"] : post["profile"]["user"]}">
                <img id="post-img" class="profile-picture text" alt="User's profile picture." src={post["buyer"] ? post["buyer"]["profile-picture"] : post["profile"]["profile-picture"]}/>
            </a>
            {#if post["buyer"]!=undefined}
                <span class="text"><a id="first-post-user" class="text username" href="/profile/{post["buyer"]["user"]}">{post["buyer"]["display"]}</a> original by <a id="post-user" class="text username" href="/profile/{post["profile"]["id"]}">{post["profile"]["display"]}</a></span>
            {:else}
                <a id="post-user" class="text username" href="/profile/{post["profile"]["user"]}">{post["profile"]["display"]}</a>
            {/if}
            <div class="post-badges-container">
                {#if post["buyer"]!=undefined}
                    {#each post["buyer"]["badges"] as badge}
                        <span class="post-badge material-symbols-outlined" title="{titles[badge]}">{badges[badge]}</span>
                    {/each}
                {:else}
                    {#each post["profile"]["badges"] as badge}
                        <span class="post-badge material-symbols-outlined" title="{titles[badge]}">{badges[badge]}</span>
                    {/each}
                {/if}
            </div>
        </div>
        <div id="post-data" class="text data"><SvelteMarkdown {source} /></div>
        <div id="post-meta" class="text timestamp" >BlockCoin Browser App - {formatDate(post["data"]["date"])}</div>
        <div id="post-stats" class="stats text">
            <span class="material-symbols-outlined view-icon">visibility</span> 
            <span class="views">{post["data"]["views"]}</span> 
            <span class="material-symbols-outlined like-button">favorite</span> 
            <span class="likes">{post["data"]["likes"]}</span> 
            <span class="material-symbols-outlined comment-button">chat</span> 
            <span class="comment">{post["data"]["comments_number"]}</span> 
            {#if post["data"]["isBuyable"]}
                <img src={bc} class="a-blockcoin" alt="The blockcoin currency!"/> 
                <span class="blockcoin">{post["data"]["price"] != 0 ? post["data"]["price"] : "Free"}</span> 
            {/if}
        </div>
    </div>
    
    <style>
        .a-blockcoin {
            width:12px;
            height:12px;
        }
        .like-button {
            transition-duration: 0.5s;
            cursor:pointer;
        }
        .comment-button:hover {
            font-size: 13px;
        }
        .comment-button {
            transition-duration: 0.5s;
            cursor:pointer;
        }
        .like-button:hover {
            font-size: 13px;
        }
        .a-blockcoin:hover {
            transition-duration: 0.5s;
            cursor:pointer;
            width:13px;
            height:13px;
        }
        .liked {
            color:#FF0000;
            font-variation-settings:
            'FILL' 1,
            'wght' 400,
            'GRAD' 0,
            'opsz' 12
        }
        #post {
            background: var(--post-background);
            width:100%;
            height: 100%;
            top:0;
            left:0;
            border: var(--post-border);
            border-radius: var(--post-radius)
        }
        .post-badge {
            font-size: var(--text-size-2);
            color: var(--badge-color);
            border: var(--badge-border);
            background: var(--badge-background);
            border-radius: var(--badge-radius);
          }
        .post-badges-container {
            width:fit-content;
            display:flex;
            flex-direction:row;
            justify-content:flex-start;
            align-items:center;
            margin:10px;
            border:var(--badge-border);
            border-radius:var(--badge-raduis);
            background: var(--badge-background);
          }
          .text {
            font-family: var(--text-font);
            font-weight: var(--text-weight-normal);
            font-size: var(--text-size-1);
            color: var(--text-color);
            word-wrap: break-word
        }
    
    
        #post-img {
            cursor:pointer;
            border-radius:256px;
            width: 48px;
            height: 48px;
            margin: 10px;
        }
    
        #post-user, #first-post-user {
            margin: 10px;
            margin-left:0;
            margin-right:0;
            cursor: var(--text-link-cursor);
            text-decoration: var(--text-link-decoration);
        }

        #post-user {
            margin-right: 5px;
        }
    
        #top-container {
            position:relative;
            top:0;
            left:0;
            width:100%;
            height:fit-content;
            display:flex;
            justify-content: start;
            align-items: center;
            flex-direction: row;
        
        }
    
        #post-meta {
            margin: 10px;
            font-weight: 300;
            font-size: var(--text-size-1);
        }
        #post-stats {
            margin: 10px;
            bottom:0;
            position:relative;
            right:0;
            text-align:right;
            font-weight: 300;
            font-size: var(--text-size-1);
        }
        #post-data {
            margin: 10px;
            white-space: pre;
        }

        .comment-button, .like-button, .view-icon {
            font-size: 12px;
        }
    </style>
