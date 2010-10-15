!SLIDE smaller
#before_filter code from action_pack#
    @@@ ruby
     def before_filter(*names, &blk)
       _insert_callbacks(names, blk) do |name, options}
         set_callback(:process_action, :before_filter, name, options)
       end
     end

