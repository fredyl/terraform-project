KeyError: 'gold_vendor_nightly.vw_CTNPF'
File <command-4178529555361027>, line 16
     13 view_sizes = get_view_sizes(enabled_views, source_table)
     15 #Processing the views
---> 16 fetch_vendor_data(enabled_views, base_path, source_table,view_sizes)
     18 #Deleting the volumes that are 2 months older than the present date
     19 volume_names = get_volumes()
File <command-4178529555361026>, line 26, in fetch_vendor_data(enabled_views, base_path, source_table, view_sizes)
     23         print(f"Saved data to {file_path_txt}")
     25 #splitting the views into smaller and larger views based on view sizes
---> 26 smaller_views = [view for view in enabled_views if view_sizes[view] < 1000000]
     27 larger_views = [view for view in enabled_views if view_sizes[view] >= 1000000]
     29 # Process smaller views in parallel
File <command-4178529555361026>, line 26, in <listcomp>(.0)
     23         print(f"Saved data to {file_path_txt}")
     25 #splitting the views into smaller and larger views based on view sizes
---> 26 smaller_views = [view for view in enabled_views if view_sizes[view] < 1000000]
     27 larger_views = [view for view in enabled_views if view_sizes[view] >= 1000000]
     29 # Process smaller views in parallel
