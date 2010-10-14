!SLIDE smaller
# supporting :only and :except options #
    @@@ ruby
    class HomeController < ApplicationController

      before_filter :load_user, :only => [:index]

    end

!SLIDE smaller
    @@@ ruby
    class HomeController < ApplicationController

      before_filter :load_user, 
        :if => proc {|c| c.action_name == 'index'}

    end



!SLIDE smaller

    @@@ ruby
    class User
      include ActiveSupport::Callbacks
      define_callbacks :validate, :terminator => "result == false"
      
      set_callback  :validate, 
        :before, 
        :index , 
        :per_key => { :if =>  lambda {self.class.name == 'User'} }

      set_callback  :validate, 
        :before, 
        :edit , 
        :per_key => { :if =>  lambda {self.class.name != 'User'} }

    end

    # User.new.send(:_run_validate_callbacks, :index)

!SLIDE smaller

    @@@ ruby
    _run__1457018200__validate__91068__callbacks

    "_run__\#{self.class.name.hash.abs}__#{symbol}" << 
                        "__\#{key.hash.abs}__callbacks"

!SLIDE smaller

    @@@ ruby
    value = nil
    halted = false

    unless halted
      result = index
      halted = (result == false)
    end

    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)

    # notice that index is direcly called here
    # no trace of method edit

!SLIDE 
#unlike other callbacks which are compiled before hand, :per_key callbacks are compiled the very first time method is invoked#

!SLIDE 
#:if/unless conditions are evaluated just one time #

!SLIDE smaller

    @@@ ruby
    value = nil
    halted = false

    unless halted
      result = edit
      halted = (result == false)
    end

    value = yield if block_given? && !halted
    halted ? false : (block_given? ? value : true)

