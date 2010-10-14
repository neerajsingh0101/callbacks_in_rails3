!SLIDE
# In Rails 3 there is ActiveModel #

!SLIDE
# Let us build an ActiveModel and declare callbacks on this model #

!SLIDE

    @@@ ruby
    class User
    
    end

 
!SLIDE small

    @@@ ruby
    class User
      
      include ActiveSupport::Callbacks
    
    end

!SLIDE small

    @@@ ruby
    class User
      
      include ActiveSupport::Callbacks

      define_callbacks :validate
    
    end

!SLIDE small

    @@@ ruby
    class User
      
      include ActiveSupport::Callbacks

      define_callbacks :validate

      set_callback :validate, :before, :check_everything
    
    end


!SLIDE small 

    @@@ ruby
    class User

      include ActiveSupport::Callbacks

      define_callbacks :validate

      set_callback :validate, :before, :check_everything

      def check_everything
        puts 'checking'
      end 
    end


!SLIDE small 

    @@@ ruby
    class User

      include ActiveSupport::Callbacks

      define_callbacks :validate

      set_callback :validate, :before, :check_everything

      def check_everything
        puts 'checking'
      end

      def validate
        self.send(:_run_validate_callbacks)
      end 
    end


!SLIDE small 

    @@@ ruby
    value = nil
    halted = false

    unless halted
      result = check_everything
      halted = (false)
    end

    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)

!SLIDE 
# try two callbacks #

!SLIDE small 

    @@@ ruby
    class User
      include ActiveSupport::Callbacks

      define_callbacks :validate

      set_callback :validate, :before, :check_everything1
      set_callback :validate, :before, :check_everything2

      def check_everything1; puts 'checking1'; end
      def check_everything2; puts 'checking2'; end
    end

!SLIDE small incremental

    @@@ ruby
    value = nil
    halted = false

    unless halted
      result = check_everything1
      halted = (false)
    end

    unless halted
      result = check_everything2
      halted = (false)
    end

    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)


!SLIDE
# Order is maintained #

# to change the order use :prepend option #

!SLIDE small

    @@@ ruby
    class User
      include ActiveSupport::Callbacks
      define_callbacks :validate

      set_callback :validate, :before,  :check_everything1

      set_callback :validate, :before,  :check_everything2, 
                                        :prepend => true

      def check_everything1; puts 'checking1'; end
      def check_everything2; puts 'checking2'; end
    end


!SLIDE small

    @@@ ruby
    value = nil
    halted = false

    unless halted
      result = check_everything2
      halted = (false)
    end

    unless halted
      result = check_everything1
      halted = (false)
    end

    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)

!SLIDE
# Notice returning false will not halt the chain in this case#

!SLIDE
# Use :terminator option if you want callback chain to halt#

!SLIDE small

    @@@ ruby
    class User
      include ActiveSupport::Callbacks

      define_callbacks  :validate, 
                        :terminator => "result == false"

      set_callback :validate, :before, :check_everything1
      set_callback :validate, :before, :check_everything2

      def check_everything1; puts 'checking1'; end
      def check_everything2; puts 'checking2'; end
    end

!SLIDE smaller

    @@@ ruby
    value = nil
    halted = false

    unless halted
      result = check_everything1
      halted = (result == false)
    end

    unless halted
      result = check_everything2
      halted = (result == false)
    end

    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)

!SLIDE smaller
#In ActionPack#
    @@@ ruby
    define_callbacks :process_action, :terminator => "response_body"

!SLIDE
#This explains why returning nil in a before_filter in a controller does not halt the callback chain#

!SLIDE
#A redirect in a before_filter will halt the callback chain#

!SLIDE
#In ActiveModel#
    @@@ ruby
    :terminator => "result == false"

!SLIDE
#returning nil in ActiveModel/ActiveRecord will halt the chain#

!SLIDE
# Adding conditions like :if or :unless to the callback #

!SLIDE smaller

    @@@ ruby
    class User
      include ActiveSupport::Callbacks

      define_callbacks  :validate, :terminator => "result == false"

      set_callback :validate, :before,  :check_everything1, 
                                        :if => "10 > 6"

      set_callback :validate, :before, :check_everything2
      def check_everything1; puts 'checking1'; end
      def check_everything2; puts 'checking2'; end
    end

!SLIDE smaller

    @@@ ruby
    value = nil
    halted = false

    if ( 10 > 6)
      unless halted
        result = check_everything1
        halted = (result == false)
      end
    end

    unless halted
      result = check_everything2
      halted = (result == false)
    end

    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)

!SLIDE
# callbacks are also applied to subclasses #

!SLIDE
# If a subclass wants to skip a callback then use skip_callback #

!SLIDE smaller

    @@@ ruby
    class User
      include ActiveSupport::Callbacks
      define_callbacks :validate, :terminator => "result == false"
      set_callback :validate, :before,  :check_everything1, 
                                        :if => " 10 > 6"

      set_callback :validate, :before,  :check_everything2, 
                                        :prepend => true

      def check_everything1; puts 'checking1'; end
      def check_everything2; puts 'checking2'; end
    end

    class Person < User
      skip_callback :validate, :before, :check_everything1
    end

!SLIDE smaller

# Compiled callback for Person #

    @@@ ruby
    value = nil
    halted = false

    unless halted
      result = check_everything2
      halted = (result == false)
    end

    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)

    # notice check_everything1 is not present

!SLIDE
# :if/:unless condition supplied to child is merged with that of parent #

!SLIDE smaller

    @@@ ruby
    class User
      include ActiveSupport::Callbacks
      define_callbacks :validate, :terminator => "result == false"

      set_callback :validate, :before,  :check_everything1, 
                                        :if => " 10 > 6"

      set_callback :validate, :before, :check_everything2

      def check_everything1; puts 'checking1'; end
      def check_everything2; puts 'checking2'; end
    end

    class Person < User
      skip_callback :validate, :before, :check_everything1, 
                                        :if => '100 > 60'
    end

!SLIDE smaller

    @@@ ruby
    value = nil
    halted = false

    unless halted
      result = check_everything2
      halted = (result == false)
    end

    if ( 10 > 6) && !(100 > 60)
      unless halted
        result = check_everything1
        halted = (result == false)
      end
    end
    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)

!SLIDE 
# if a proc is used as target then compiled method calls a dynamically created method #

!SLIDE smaller

    @@@ ruby
    class User
      include ActiveSupport::Callbacks

      define_callbacks :validate, :terminator => "result == false"

      set_callback :validate, :before, lambda {puts 'done'}

    end

!SLIDE smaller

    @@@ ruby
    value = nil
    halted = false

    unless halted
      result = _callback_before_7
      halted = (result == false)
    end

    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)


