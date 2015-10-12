#########################################
#  CSA Operations/Audit Plugin 	        #
#  Version 0.04 						#
#########################################

This plugin allows a "Super Admin" to manage Subscriptions across multiple users and organisations efficiently.

#########################################
#  Installation Instructions 			#
#########################################

1. Create the folder custom-content in directory <csahome>/jboss-as/standalone/csa.war
2. Extract the content of audit.zip into the custom-content folder, observe the correct folder structure in the custom-content folder as below:

	#############################################
	#  File Contents / Folder Structure 		#
	#############################################
	# 											#
	# audit/index.jsp 							#
	# audit/setup.json 							#
	# audit/getSubs.jsp 						#
	# audit/action.jsp 							#
	# audit/README.md  							#
	# audit/css/audit.css 						#
	# audit/js/audit.js 						#
	#											#
	# assets/css/datatables.bootstrap.css 		#
	# assets/css/dataTables.responsive.css 		#
	# assets/css/dataTables.fixedHeader.css 	#
	#											#
	# assets/js/dataTables.bootstrap.js 		#
	# assets/js/dataTables.responsive.js 		#
	# assets/js/dataTables.fixedHeader.min.js 	#
	# assets/js/jquery.dataTables.min.js 		#
	#											#
	#############################################

3. Add the following entry to the csa.war/dashboard/config.json (inside main.tiles array or in sub panel see Configuration guide if unsure how to manipulate this file)

	{
		"id": "audit",
		"name": "audit",
		"description": "audit_description",
		"enabled": true,
		"style": "custom-tile-header",
		"target": "iframe",
		"data": "/csa/custom-content/audit/",
		"helptopic": "console_help",
		"roles": ["CSA_ADMIN"]
	}

4. Open the file csa.war/dashboard/messages/common/messages.properties and location section entitled:
	# Page titles and descriptions, used for the dashboard tiles and for navigation views

Add the following entries
		
	audit=Audit / Operations
	audit_description=This experimental feature allows a Super User to Manage Subscriptions across all organisations Efficiently (Experimental)

5. To fix the CSA styling on the dashboard you can add the following to the end of the file csa.war/dashboard/css/base.css to make the Tile Title fit better.

	.tile h3 {
		white-space:inherit;
		font-size:1.4em;
	}

5. Configure the settings in csa.war/custom-content/audit/setup.json

	MPP_HOST 					- The url of the MPP instance  e.g https://W2K12CSA420OO:8089/
	DATA_URL 					- The path to the URL for retrieving the Subscriptions default. getSubs.jsp
	ENABLE_CONSUMER_ADMIN_LINKS - Set as false disable direct links to Manage Subscriptions as Consumer admin
	ENABLE_CANCEL_LINKS 		- Set as false to disable cancel subscription functionality
	ENABLE_RESUME_LINKS			- Set as false to disable resuming paused subscription functionality
	ENABLE_DELETE_LINKS			- Set as false to disable deleting offline subscription functionality
	REQUIRE_CONFIRMATION		- Set as false to determine the default behaviour regarding confirmation prompts
	SHOW_RETIRED				- Set as false to only show active artifacts (Can be overriden on Load)
	DEFAULT_DISPLAY_LENGTH		- How many rows should be displayed (Default 25, Possible values 10,25,50 or ALL)
	USE_FIXED_HEADER			- Set as false to disable the fixed header behaviour
	CONFIG_CACHE				- Integer representing the number of days end user configuration remain in browser cache (Default 5)
	SEARCH_TERM					- The default value set in the Datatables search field (Default Empty String)
	VISIBLE						- An object array of Data which is shown in the sortable table, true = in table, false = in detail.



#############################################
#	Known Issues							#
#############################################

1. If not signed into MPP organisation then Consumer Admin links may redirect to incorrect login screen (needs some attention)
2. After session timeout Refresh request on Datatable throws json error instead of redirecting to login.
3. No visual feedback on refresh action (unless the data response has changed)
4. FixedHeader sometimes doesn't align to column widths


#############################################
# Release notes v.0.2						#
#############################################
- Updated README.md
	- File Path corrections
	- Added Step about Dashboard CSS
	- Updated Tile Title (to Include Experimental)
- Fixed Delete Action URL path (now is relative)
- New Render for Retired Column to avoid filter clash with Subscription Status. (Active renamed to "Not Retired")
- Public Action Failed can now be cancelled.
- Updated Fullscreen to use iframe overlay instead of new page load (To prevent lose of filter)
- Removed NoCache include.
- Added colVis plugin to customize which columns are displayed.
- Added CACHE_CONFIG setup parameter (for storing Cached configuration setup)
- Removed SHOW_ICON Setup (deprecated due to ColVis plugin)
- Added DEFAULT_DISPLAY_LENGTH to setup 
- Added VISIBLE config attribute to set the default columns which are visible.
- Add StyleRule for Icons when displyed in the row detail

#############################################
# Release notes v.0.3						#
#############################################
- Add Restore Settings button which will clear any cached configuration and reload the datatable.
- Added SEARCH_TERM to the setup config

#############################################
# Release notes v.0.4						#
#############################################
- Typo in Delete Sub condition.
- Updated Default visible config to include user.
- Updated Default Retired to be hidden.