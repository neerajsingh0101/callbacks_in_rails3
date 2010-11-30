!SLIDE
#Quiz#

!SLIDE
    @@@ ruby
    class User < ActiveRecord::Base
      before_save :check_permission1
      before_save :check_permission2

      def check_permission1
        return false
      end

      def check_permission2
        puts "check_permission2"
      end
    end

!SLIDE
#callback will _be_ halted#


!SLIDE
    @@@ ruby
    class User < ActiveRecord::Base
      before_save :check_permission1
      before_save :check_permission2

      def check_permission1
        return nil
      end

      def check_permission2
        puts "check_permission2"
      end
    end

!SLIDE
#callback will _NOT_ be halted#

!SLIDE
    @@@ ruby
    class UsersController < ApplicationController
      before_filter :check_permission1
      before_filter :check_permission2

      def check_permission1
        return nil
      end

      def check_permission2
        puts "check_permission2"
      end
    end

!SLIDE
#callback will _NOT_ be halted#


!SLIDE
    @@@ ruby
    class UsersController < ApplicationController
      before_filter :check_permission1
      before_filter :check_permission2

      def check_permission1
        return false
      end

      def check_permission2
        puts "check_permission2"
      end
    end

!SLIDE
#callback will _NOT_ be halted#


!SLIDE
#By the end this presentation you would know why it is the way it is#

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
