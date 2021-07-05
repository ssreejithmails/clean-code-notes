# clean-code-notes

# DI Infracode
class Service {
  @Autowired UserDetailsFinder finder;
}
class LdapQuery implements UserDetailsFinder{

