!SLIDE small 

# Rails 2 #

    @@@ ruby
    class UsersController < ApplicationController

      before_filter :load_user, :only => [:show], 
                                :if => '10 > 6'

      before_filter :check_permission,  :except => [:index], 
                                        :unless => '10 > 6'

    end

!SLIDE bullets incremental

# Rails 2 #

* check for all the filters list
* go through :only and :except to determine all the filters that apply
* run through the conditions (:if/:unless)
* apply those filters

!SLIDE

# And do that again and again every single time a request is processed #

!SLIDE incremental

# Rails 3 #

# compiles the filters into a method #

!SLIDE bullets incremental

# Rails 2 callbacks code spread between #

* ActiveSupport
* ActionPack
* ActiveRecord

!SLIDE 
# Rails3 callbacks code consolidated at ActiveSupport #
