# MyBB_Theme_COD

COD v1.2
» About:

This is a simple dark theme for MyBB enthusiasts styled for Call of Duty fans.

COD v1.2
» Installation:

1.) After downloading the Theme unpack it (with 7-Zip for example)
2.) Upload the contents of "Upload" into your forums main directory. *

* if prompted to overwite index.php select "yes" as there are edits made to the index for threadlist display.
3.) Go to the Admin Control Panel under Themes > Import
4.) Search for the XML File titled "cod-theme.xml" in the theme folder found on your computer and click the Button "Import Theme" this will upload the file from your computer to your forum.
5.) Now have fun with your forum!

COD v1.2
» License:

You may not remove or change the MyBB copyright nor the Designed by: "Vintagedaddyo" statements in the footer of this theme's templates. You may use and modify this theme to your personal likings, but redistributing a modified version for download is prohibited, unless you have explicit written permission from "Vintagedaddyo", though you are allowed to redistribute a copy that has not been modified.



COD v1.2
» Changelog:

History:

07/11/2019 — Theme updated for 1.8.21
04/05/2019 — Theme updated for 1.8.20
09/11/2018 — Theme updated for 1.8.19
08/30/2018 — Theme updated for 1.8.18
09/29/2016 — Initial Theme Creation

NOTE when updating mybb versions:

After you update your mybb forums version, the included modified index.php in cod theme will be overwritten so you will need to re-modify the new index.php file.

In index.php find:


$plugins->run_hooks('index_start');



Add the following directly after:

// start threadlist on index

// get forums user cannot view
$unviewable = get_unviewable_forums(true);
if($unviewable)
{
        $unviewwhere = " AND fid NOT IN ($unviewable)";
}

        $altbg = alt_trow();
        $threadlist = '';
        $query = $db->query("
                SELECT t.*, u.username
                FROM ".TABLE_PREFIX."threads t
                LEFT JOIN ".TABLE_PREFIX."users u ON (u.uid=t.uid)
                WHERE 1=1 $unviewwhere AND t.visible='1' AND t.closed NOT LIKE 'moved|%'
                ORDER BY t.lastpost DESC
                LIMIT 0, ".$mybb->settings['portal_showdiscussionsnum']
        );
        while($thread = $db->fetch_array($query))
        {
                $lastpostdate = my_date($mybb->settings['dateformat'], $thread['lastpost']);
                $lastposttime = my_date($mybb->settings['timeformat'], $thread['lastpost']);
                // Don't link to guest's profiles (they have no profile).
                if($thread['lastposteruid'] == 0)
                {
                        $lastposterlink = $thread['lastposter'];
                }
                else
                {
                        $lastposterlink = build_profile_link($thread['lastposter'], $thread['lastposteruid']);
                }
                if(my_strlen($thread['subject']) > 25)
                {
                        $thread['subject'] = my_substr($thread['subject'], 0, 25) . "...";
                }
                $thread['subject'] = htmlspecialchars_uni($parser->parse_badwords($thread['subject']));
                $thread['threadlink'] = get_thread_link($thread['tid']);
                $thread['lastpostlink'] = get_thread_link($thread['tid'], 0, "lastpost");
                eval("\$threadlist .= \"".$templates->get("portal_latestthreads_thread")."\";");
                $altbg = alt_trow();
        }
        if($threadlist)
        {
                // Show the table only if there are threads
                eval("\$latestthreads = \"".$templates->get("portal_latestthreads")."\";");
        } 

// end threadlist on index
